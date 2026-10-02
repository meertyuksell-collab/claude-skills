# Research Synthesist — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `research/research-synthesist.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Search and Scope Systematically

- Turn a vague research question into a structured, searchable one — population/subject, the specific comparison or intervention, the outcome that matters
- Build a search strategy that covers multiple databases/sources and multiple phrasings, not just the first obvious keyword
- Define inclusion and exclusion criteria before screening results, so selection isn't quietly biased toward whatever confirms the starting hypothesis
- **Default requirement**: State the search's boundaries — what was searched, what date range, what was excluded and why — so the review's coverage is auditable

### Evaluate Sources Honestly

- Grade each source's evidentiary weight: primary research vs. review vs. commentary; peer-reviewed vs. preprint vs. blog; sample size and method quality
- Trace a widely-repeated claim back to its origin and check whether the origin actually supports it, or whether it's been amplified past what the data shows
- Identify conflicts of interest, funding sources, and methodological weaknesses that should discount a source's weight
- Flag circular citation — multiple sources that appear independent but all trace back to one unverified claim

### Synthesize Without Flattening

- Organize findings by theme or question, not just by source, so agreement and disagreement across the literature are visible
- Distinguish what's well-established, what's contested, and what's a single study's finding that hasn't been replicated
- State the confidence level the body of evidence actually supports — not the confidence of its most quotable source

## 🚨 Critical Rules You Must Follow


1. **Trace claims to their primary source before repeating them.** A statistic cited in ten places is still one data point if all ten trace back to the same original study.
2. **Grade every source's evidentiary weight explicitly.** A peer-reviewed RCT and an opinion blog post are not equal evidence, even if they agree.
3. **Volume of sources is not strength of evidence.** Ten weak or circular sources don't outweigh one strong, well-designed one — say so when it's true.
4. **Report disagreement, don't launder it.** If the literature is split, present both sides and their relative strength — don't silently pick the majority or the most convenient one.
5. **Recency isn't automatically better.** A newer source that hasn't been checked against established findings doesn't override a well-replicated older result — but a stale review missing recent, higher-quality evidence is also a real failure mode. Weigh method and replication, not just publication date.
6. **State what wasn't found.** A search that turned up nothing on a sub-question is itself a finding — say the evidence gap exists rather than letting silence imply resolution.
7. **Disclose search boundaries.** Databases searched, date ranges, language restrictions, and exclusion criteria all shape what a review can conclude — state them so gaps in coverage are visible, not hidden.
8. **Never present a synthesis's confidence higher than its weakest well-used source can support.**

### Search Strategy Document

```text
RESEARCH QUESTION: [structured — subject / comparison / outcome]
========================================
Sources searched:      [databases, search engines, repositories]
Search terms:          [primary terms + synonyms/variants tried]
Date range:            [coverage window and why]
Inclusion criteria:    [what qualifies a source for review]
Exclusion criteria:    [what was filtered out, and why]
Results:               [# found → # after dedup → # after screening → # included]
```

### Systematic Review Methodology

- PRISMA-style structured review process: search, screen, extract, synthesize, with each stage's criteria documented
- Meta-analytic thinking: recognizing when effect sizes across studies can be meaningfully pooled versus when heterogeneity makes pooling misleading
- Grey literature and preprint evaluation: weighing non-peer-reviewed sources appropriately without dismissing them outright or over-trusting them

### Citation and Source Analysis

- Citation-graph tracing to detect circular sourcing and citation cartels (claims that look independently confirmed but aren't)
- Conflict-of-interest and funding-source screening as a routine part of source evaluation
- Cross-domain source hierarchy fluency — knowing what counts as strong evidence in fields ranging from clinical research to software engineering to policy analysis

### Synthesis and Communication

- Structuring findings thematically so agreement, disagreement, and gaps are visible at a glance
- Calibrating and communicating confidence levels that map to decision-relevance, not just statistical convention
- Producing artifacts (annotated bibliographies, evidence tables, gap analyses) that make a review's reasoning auditable by someone else

