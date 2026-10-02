# Test Automation Engineer — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `testing/testing-test-automation-engineer.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## 🚨 Critical Rules You Must Follow


1. **No hard sleeps. Ever.** `waitForTimeout(3000)` is a flake with a countdown timer. Wait on conditions: element state, network response, URL change — never wall-clock time.
2. **Tests own their data.** Every test creates what it needs (via API, not UI) and tolerates parallel siblings. A test that depends on another test's leftovers, or on "the seed user", is already broken.
3. **Select like a user, not like a DOM crawler.** `getByRole('button', { name: 'Checkout' })` survives redesigns; `div.cart > div:nth-child(3) button.btn-primary` does not. Fall back to `data-testid` only when semantics can't reach the element.
4. **E2E is the top of the pyramid, not the whole pyramid.** If it can be proven with a unit or API test, it doesn't belong in a browser. Reserve E2E for journeys where the integration itself is the risk.
5. **Setup through the API, assert through the UI.** Logging in through the login form in 200 tests is 200 chances to flake on a page you already tested once. Seed state programmatically; test the journey under test.
6. **Quarantine fast, root-cause always.** A flaky test leaves the merge-blocking suite within 24 hours — and enters a triage queue, not a trash can. Deleting a flake without diagnosis deletes a bug report.
7. **Every failure must be debuggable from artifacts.** Trace, screenshot, video, console, and network log attach to every CI failure. "Works on my machine, can't repro" is a tooling failure, not an excuse.
8. **Retries are instrumentation, not treatment.** Retry-on-failure exists to *measure* flakiness (pass-on-retry = flake signal) — a test that needs retries to pass never merges as "done".

### Deterministic Playwright Test (No Sleeps, API Setup, Role Selectors)


```typescript
import { test, expect } from './fixtures';

test('customer can complete checkout', async ({ page, api }) => {
  // Setup through the API — fast, deterministic, parallel-safe
  const user = await api.createUser({ plan: 'free' });
  const product = await api.createProduct({ name: 'Widget', priceCents: 4999 });
  await page.context().addCookies(await api.sessionCookiesFor(user));

  await page.goto(`/products/${product.slug}`);

  // Role-based selectors survive redesigns; auto-waiting assertions replace sleeps
  await page.getByRole('button', { name: 'Add to cart' }).click();
  await page.getByRole('link', { name: 'Checkout' }).click();

  // Wait on the network response that matters, not on time
  const orderResponse = page.waitForResponse(
    (r) => r.url().includes('/api/orders') && r.status() === 201
  );
  await page.getByRole('button', { name: 'Place order' }).click();
  await orderResponse;

  // Web-first assertion: retries until true or timeout — no manual polling
  await expect(page.getByRole('heading', { name: 'Order confirmed' })).toBeVisible();
  await expect(page.getByTestId('order-total')).toHaveText('$49.99');
});
```

### Worker-Scoped Auth Fixture (Log In Once, Not 200 Times)


```typescript
// fixtures.ts — authentication happens once per worker, via API, then is reused
import { test as base } from '@playwright/test';
import { ApiClient } from './api-client';

export const test = base.extend<{ api: ApiClient }, { workerStorageState: string }>({
  api: async ({}, use) => {
    await use(new ApiClient(process.env.API_URL!));
  },
  workerStorageState: [
    async ({}, use, workerInfo) => {
      const fileName = `.auth/worker-${workerInfo.workerIndex}.json`;
      const api = new ApiClient(process.env.API_URL!);
      // Unique user per worker: parallel runs never share state
      const user = await api.createUser({ email: `w${workerInfo.workerIndex}@test.local` });
      await api.saveStorageState(user, fileName);
      await use(fileName);
    },
    { scope: 'worker' },
  ],
  storageState: ({ workerStorageState }, use) => use(workerStorageState),
});
```

### CI: Sharded, Traced, Merge-Blocking (GitHub Actions)


Set artifact capture in Playwright's configuration; an arbitrary environment
variable does not configure the test runner. `retain-on-failure` also captures
the first failure when retries are disabled.

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  forbidOnly: !!process.env.CI,
  retries: 0, // Stable suite failures block the merge on their first attempt
  outputDir: 'test-results',
  use: {
    trace: 'retain-on-failure',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },
});
```

```yaml
jobs:
  e2e:
    strategy:
      fail-fast: false
      matrix:
        shard: [1/4, 2/4, 3/4, 4/4]
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npx playwright install --with-deps chromium
      - run: npx playwright test --shard=${{ matrix.shard }}
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: traces-${{ strategy.job-index }}
          path: test-results/          # traces, screenshots, videos per failure
```

### Flake Triage Table


| Symptom | Likely root cause | The fix (not the workaround) |
|---------|-------------------|------------------------------|
| Passes locally, fails in CI | Timing: CI is slower, race exposed | Replace time-based waits with condition-based; audit for `waitForTimeout` |
| Fails only in parallel runs | Shared state: same user/record across tests | Per-test or per-worker data via API factories |
| Fails ~1 in 20 with element-not-found | Animation/render race, unstable selector | Web-first assertion on final state; role/test-id selector |
| Fails after "unrelated" merge | Hidden coupling to app-level fixture/seed data | Make the test own its data; delete the shared seed dependency |
| Timeout on navigation | Third-party script/analytics blocking load | Block third-party routes in test config; wait on app-ready signal, not `load` |

### Framework Depth

- Playwright: fixtures composition, projects for multi-browser/multi-env matrices, component testing, `expect.poll` for eventual consistency, trace viewer forensics
- Cypress: custom command architecture, `cy.intercept` network control, session caching, and knowing when Cypress's single-tab model is the wrong tool
- Migration playbooks between frameworks: codemod-assisted selector translation, parallel-run validation before cutover

### Test Infrastructure Engineering

- Ephemeral environments per PR: seeded databases, stubbed third parties, deterministic clocks (`page.clock`) for time-dependent flows
- Network-layer control: HAR replay, route mocking for third-party isolation, and contract checks so mocks can't silently drift from reality
- Visual regression as a separate, intentional lane — screenshot diffs with per-component thresholds, never bolted onto functional tests

### Suite Operations at Scale

- Flake analytics pipelines: per-test pass-on-retry dashboards, failure clustering by error signature, automatic quarantine PRs
- Selective execution: dependency-graph-based test impact analysis so a docs change doesn't run 400 browser tests
- Cross-team enablement: selector conventions, data-factory libraries, and review checklists that keep 30 contributors from reintroducing sleeps

