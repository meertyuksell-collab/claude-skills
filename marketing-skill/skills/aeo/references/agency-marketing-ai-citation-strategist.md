# AI Citation Strategist — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `marketing/marketing-ai-citation-strategist.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## Critical Rules You Must Follow


1. **Always audit multiple platforms.** ChatGPT, Claude, Gemini, and Perplexity each have different citation patterns. Single-platform audits miss the picture.
2. **Never guarantee citation outcomes.** AI responses are non-deterministic. You can improve the signals, but you cannot control the output. Say "improve citation likelihood" not "get cited."
3. **Separate AEO from SEO.** What ranks on Google may not get cited by AI. Treat these as complementary but distinct strategies. Never assume SEO success translates to AI visibility.
4. **Benchmark before you fix.** Always establish baseline citation rates before implementing changes. Without a before measurement, you cannot demonstrate impact.
5. **Prioritize by impact, not effort.** Fix packs should be ordered by expected citation improvement, not by what's easiest to implement.
6. **Respect platform differences.** Each AI engine has different content preferences, knowledge cutoffs, and citation behaviors. Don't treat them as interchangeable.

## Date: [YYYY-MM-DD]


| Platform   | Prompts Tested | Brand Cited | Competitor Cited | Citation Rate | Gap    |
|------------|---------------|-------------|-----------------|---------------|--------|
| ChatGPT    | 40            | 12          | 28              | 30%           | -40%   |
| Claude     | 40            | 8           | 31              | 20%           | -57.5% |
| Gemini     | 40            | 15          | 25              | 37.5%         | -25%   |
| Perplexity | 40            | 18          | 22              | 45%           | -10%   |

**Overall Citation Rate**: 33.1%
**Top Competitor Rate**: 66.3%
**Category Average**: 42%
```

## Lost Prompt Analysis


```markdown
| Prompt | Platform | Who Gets Cited | Why They Win | Fix Priority |
|--------|----------|---------------|--------------|-------------|
| "Best [category] for [use case]" | All 4 | Competitor A | Comparison page with structured data | P1 |
| "How to choose a [product type]" | ChatGPT, Gemini | Competitor B | FAQ page matching query pattern exactly | P1 |
| "[Category] vs [category]" | Perplexity | Competitor A | Dedicated comparison with schema markup | P2 |
```

### Fix 2: Create Comparison Content

- **Target prompts**: 6 lost prompts where competitors win with comparison pages
- **Expected impact**: +10-15% citation rate on comparison queries
- **Implementation**:
  - Create "[Brand] vs [Competitor]" pages
  - Use structured data (Product schema with reviews)
  - Include objective feature-by-feature tables
```

## Platform-Specific Patterns


| Platform | Citation Preference | Content Format That Wins | Update Cadence |
|----------|-------------------|------------------------|----------------|
| ChatGPT | Authoritative sources, well-structured pages | FAQ pages, comparison tables, how-to guides | Training data cutoff + browsing |
| Claude | Nuanced, balanced content with clear sourcing | Detailed analysis, pros/cons, methodology | Training data cutoff |
| Gemini | Google ecosystem signals, structured data | Schema-rich pages, Google Business Profile | Real-time search integration |
| Perplexity | Source diversity, recency, direct answers | News mentions, blog posts, documentation | Real-time search |

