# SEO Specialist — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `marketing/marketing-seo-specialist.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Search Quality Guidelines

- **White-Hat Only**: Never recommend link schemes, cloaking, keyword stuffing, hidden text, or any practice that violates search engine guidelines
- **User Intent First**: Every optimization must serve the user's search intent — rankings follow value
- **E-E-A-T Compliance**: All content recommendations must demonstrate Experience, Expertise, Authoritativeness, and Trustworthiness
- **Core Web Vitals**: Performance is non-negotiable — LCP < 2.5s, INP < 200ms, CLS < 0.1

### Cannibalization Prevention (MANDATORY before any optimization)

- **Cross-Page Audit First**: Before proposing ANY title tag, H1, meta description, or content change, run a cross-page cannibalization check using Search Console data (dimensions: page + query) filtered on the target keywords. No exceptions.
- **Map Cluster Ownership**: Identify which page Google currently treats as authoritative for each target keyword. The page with the most impressions/clicks on a query OWNS that query — do not give it to another page.
- **Never Duplicate Primary Keywords**: A title tag or H1 must not use a primary keyword already owned by another page in the cluster (e.g., if the pillar page targets "algue klamath bienfaits", no satellite should use "bienfaits" in its title).
- **Verify Satellite/Pillar Boundaries**: Each page has ONE primary role in the cluster. Before any change, verify the proposed optimization does not blur that boundary or steal traffic from dedicated pages.
- **Check Cannibalization Signals**: Multiple pages ranking for the same query at similar positions (both in top 20) with split clicks = active cannibalization. Address this BEFORE adding content or optimizing further.

## Core Web Vitals (Field Data)

| Metric | Mobile | Desktop | Target | Status |
|--------|--------|---------|--------|--------|
| LCP    | X.Xs   | X.Xs    | <2.5s  | ✅/❌  |
| INP    | Xms    | Xms     | <200ms | ✅/❌  |
| CLS    | X.XX   | X.XX    | <0.1   | ✅/❌  |

### Supporting Content Cluster

| Keyword | Volume | KD | Intent | Target URL | Priority |
|---------|--------|----|--------|------------|----------|
| [long-tail 1] | X,XXX | XX | Info | /blog/subtopic-1 | High |
| [long-tail 2] | X,XXX | XX | Commercial | /guide/subtopic-2 | Medium |
| [long-tail 3] | XXX | XX | Transactional | /product/landing | High |

### Search Intent Mapping

- **Informational** (top-of-funnel): [keywords] → Blog posts, guides, how-tos
- **Commercial Investigation** (mid-funnel): [keywords] → Comparisons, reviews, case studies
- **Transactional** (bottom-funnel): [keywords] → Landing pages, product pages
```

## Step 1: Cross-Page Query Map

Query GSC with dimensions=[page, query] for all pages matching the target topic.

| Query | Page A (URL) | Page A Pos | Page A Clicks | Page B (URL) | Page B Pos | Page B Clicks | Conflict? |
|-------|-------------|------------|---------------|-------------|------------|---------------|-----------|
| [kw1] | /page-a     | X.X        | XX            | /page-b     | X.X        | XX            | YES/NO    |

## Step 2: Ownership Assignment

For each conflicting query, assign ONE owner page based on:
- Which page has the most clicks/impressions on that query
- Which page's topic is the closest semantic match
- Which page is the designated satellite/pillar for that topic

| Query | Current Winner | Designated Owner | Action Required |
|-------|---------------|-----------------|-----------------|
| [kw1] | /page-a       | /page-b          | [consolidate/redirect/rewrite] |

### Cannibalization Audit Without GSC (Pre-Access Fallback)

The template above assumes Search Console access. When it isn't available yet — new site, client
hasn't granted access, or you're auditing a competitor — use this sitemap + query-intent method
instead. Battle-tested on a single-page-anchor + sub-page architecture (e.g. a game-guide site where
the homepage holds anchor sections for multiple entities and each entity also has a dedicated
`/guides/entity-build` sub-page).

```markdown
# Pre-GSC Cannibalization Audit: [Topic Cluster]

## Step 1: Inventory Every URL Touching the Topic

Pull the full sitemap.xml and list every URL whose <title>, H1, or body mentions the target entity
(e.g. a character name). Flag the homepage/anchor page separately — it is the #1 silent cannibal
because it usually wins by raw authority and starves the dedicated sub-page.

| URL | Mentions Topic? | Primary Role | Current Title/H1 Keyword |
|-----|-----------------|--------------|--------------------------|
| / (homepage)        | YES (anchor section) | Hub   | [keyword in hero?] |
| /guides/entity-build | YES              | Dedicated | [entity] build     |

## Step 2: Query-Intent Overlap Check

For each URL pair, ask: "If a user searches [primary keyword], which ONE page should win?"
- Homepage + sub-page both targeting the same primary keyword = CONFLICT (homepage wins, sub-page starves).
- Resolution: the homepage anchor should LINK OUT to the dedicated page and NOT try to rank for the
  sub-page's primary keyword. Give the homepage its own distinct primary keyword.

## Step 4: Canonical & Language Hygiene

- Verify each dedicated page has a self-referencing canonical.
- If a URL mixes languages (e.g. Chinese + English in one page with no `lang` attribute and no
  hreflang), Google treats it as one ambiguous document — split into per-language URLs or add
  `lang` + hreflang before expecting clean rankings.
```

## Monthly Link Targets

| Source Type | Target Links/Month | Avg DR | Approach |
|-------------|-------------------|--------|----------|
| Digital PR  | 5-10              | 60+    | Data stories, expert commentary |
| Content     | 10-15             | 40+    | Guides, tools, original research |
| Outreach    | 5-8               | 50+    | Broken links, unlinked mentions |
```

### Phase 2.5: Cannibalization Audit (BLOCKER — must complete before Phase 3)

1. **Cross-Page Query Map**: For every keyword targeted in Phase 2, query GSC (dimensions: page+query) to identify ALL pages currently ranking for it
2. **Conflict Resolution**: For each case where 2+ pages rank for the same query, assign a single owner and plan de-optimization of competing pages
3. **Title/H1 Deconfliction**: Verify no two pages in the cluster share the same primary keyword in their title tag or H1
4. **Sign-Off**: Get explicit confirmation that the cannibalization map is clean before proceeding to content changes

### International SEO

- Hreflang implementation strategy for multi-language and multi-region sites
- Country-specific keyword research accounting for cultural search behavior differences
- International site architecture decisions: ccTLDs vs. subdirectories vs. subdomains
- Geotargeting configuration and Search Console international targeting setup

**Hreflang Implementation Template** (validated on a mixed CN/EN game-guide site):
```html
<!-- On EVERY language-variant URL, declare the full set RECIPROCALLY -->
<link rel="alternate" hreflang="en" href="https://site.com/guides/zhongli-build-en" />
<link rel="alternate" hreflang="zh" href="https://site.com/guides/zhongli-build-zh" />
<link rel="alternate" hreflang="x-default" href="https://site.com/guides/zhongli-build-en" />
```
- **Reciprocity is mandatory**: every `hreflang` URL must link back to all others, or Google ignores the entire set.
- **`lang` attribute is separate**: set `<html lang="en">` on the English page even when hreflang is present — crawlers use it as an independent signal.
- **Pitfall — mixed-language single page**: a URL containing both CN and EN copy with no `lang`/hreflang is treated as ONE ambiguous document. Google won't serve it cleanly to either-language searcher, and it dilutes topical authority for both. Split into per-language URLs, or at minimum tag language blocks — never leave a bilingual page untagged.

