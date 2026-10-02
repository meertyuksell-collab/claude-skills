# AEO Foundations Architect — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `marketing/marketing-aeo-foundations.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## 🚨 Critical Rules


1. **Audit foundations before optimizations.** Never recommend citation fixes, content restructuring, or WebMCP implementation until the discovery and parsability layer is verified. Foundations first.
2. **Never block AI crawlers by default.** The default posture should be allowing AI crawlers unless the business has a specific, documented reason to block. Blocking by ignorance (unchanged legacy robots.txt) is the most common AEO failure.
3. **Respect content licensing decisions.** Some businesses have legitimate reasons to block AI training crawlers (GPTBot, ClaudeBot) while allowing search-augmented crawlers (PerplexityBot, Google-Extended). Present the options clearly, implement the business decision, don't make the decision.
4. **Token budgets are hard constraints, not guidelines.** AI systems have finite context windows. Content that exceeds token budgets gets truncated, summarized lossy, or skipped entirely. Treat token limits as seriously as page load time budgets.
5. **Test with real AI systems, not assumptions.** After implementing llms.txt or robots.txt changes, verify by querying AI systems and checking crawl logs. "I published it" is not the same as "AI systems found it."
6. **Keep discovery files maintained.** Publishing llms.txt once and forgetting it is worse than not having one — stale discovery files point AI to dead pages and outdated content.

### 1. Discovery Layer

| Check                          | Status | Detail                              |
|--------------------------------|--------|-------------------------------------|
| robots.txt has AI crawler rules| ❌ No  | No mention of GPTBot, ClaudeBot, etc|
| llms.txt published             | ❌ No  | /llms.txt returns 404               |
| llms-full.txt published        | ❌ No  | /llms-full.txt returns 404          |
| AGENTS.md at repo root         | N/A    | No public repo                      |
| Sitemap includes content pages | ✅ Yes | 142 URLs in sitemap.xml             |
| AI crawl activity in logs      | ⚠️ Partial | GPTBot seen, blocked by robots.txt |

### 2. Parsability Layer

| Check                          | Status | Detail                              |
|--------------------------------|--------|-------------------------------------|
| Key pages available as clean HTML | ⚠️ Partial | Blog: yes. Product pages: JS-rendered |
| Markdown alternatives available| ❌ No  | No /api/content or .md endpoints    |
| Average content length (tokens)| ⚠️ High | Homepage: 38K tokens (target: <15K) |
| Heading hierarchy (H1→H6)     | ✅ Yes | Clean semantic structure             |
| FAQ schema on key pages        | ❌ No  | 0/12 target pages have FAQPage      |

### 3. Capability Layer

| Check                          | Status | Detail                              |
|--------------------------------|--------|-------------------------------------|
| agent-permissions.json         | ❌ No  | Not published                       |
| WebMCP discovery endpoint      | ❌ No  | No /mcp-actions.json                |
| Structured action declarations | ❌ No  | No data-mcp-action attributes       |

**Foundation Score: 2/12 (17%)**
**Target (30-day): 9/12 (75%)**
```

### Token Budget Worksheet


```markdown
# Token Budget Analysis: [Site Name]

| Content Type    | Target Budget | Current Avg | Status   | Action                           |
|-----------------|--------------|-------------|----------|----------------------------------|
| Quick Start     | <15,000 tok  | 8,200 tok   | ✅ Pass  | None                             |
| How-To Guide    | <20,000 tok  | 34,500 tok  | ❌ Over  | Split into 3 focused guides      |
| Landing Page    | <8,000 tok   | 6,300 tok   | ✅ Pass  | None                             |
| Blog Post       | <12,000 tok  | 18,700 tok  | ❌ Over  | Add TL;DR section, trim examples |

### Token Estimation Method

- Tool: tiktoken (cl100k_base encoding) or LLM tokenizer
- Count includes: visible text, alt attributes, structured data, navigation
- Count excludes: CSS, JavaScript, HTML boilerplate, tracking scripts
```

### [Topic 1]

- [Page Title](/url): [Description] — [token count estimate]
```

For the full llms.txt specification and examples, see [llms-txt.cloud](https://llms-txt.cloud/) and Jeremy Howard's [original proposal](https://www.answer.ai/posts/2024-09-03-llmstxt.html).

### Content Availability Tiers


| Tier | Format | AI Accessibility | Use For |
|------|--------|-----------------|---------|
| Tier 1 | llms.txt + Markdown endpoints | Highest — direct ingestion | Core product pages, docs, FAQ |
| Tier 2 | Clean semantic HTML + schema | High — easy parsing | Blog posts, guides, landing pages |
| Tier 3 | Server-rendered HTML (no JS) | Medium — parseable but noisy | Dynamic listings, catalogs |
| Tier 4 | JS-rendered SPA content | Low — requires headless rendering | Dashboards, interactive tools |
| Tier 5 | PDF-only or image-based | Minimal — lossy extraction | Legacy docs (migrate to Tier 1-2) |

### Wave 3 (Agentic Task Completion) Prerequisites

- [ ] agent-permissions.json published
- [ ] /mcp-actions.json endpoint live (or planned)
- [ ] Key task flows use native HTML forms (not JS-only widgets)
- [ ] Guest flows available (no mandatory auth for first interaction)
```

