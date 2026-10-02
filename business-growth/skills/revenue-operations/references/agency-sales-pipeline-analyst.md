# Pipeline Analyst — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `sales/sales-pipeline-analyst.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Forecasting Methodology

Move beyond simple stage-weighted probability. Rigorous forecasting layers multiple signal types:

**Historical Conversion Analysis**: What percentage of deals at each stage, in each segment, in similar time periods, actually closed? This is your base rate — and it is almost always lower than the probability your CRM assigns to the stage.

**Deal Velocity Weighting**: Deals progressing faster than average have higher close probability. Deals progressing slower have lower. Adjust stage probability by velocity percentile.

**Engagement Signal Adjustment**: Active deals with multi-threaded stakeholder engagement close at 2-3x the rate of single-threaded, low-activity deals at the same stage. Incorporate this into the model.

**Seasonal and Cyclical Patterns**: Quarter-end compression, budget cycle timing, and industry-specific buying patterns all create predictable variance. Your model should account for them rather than treating each period as independent.

**AI-Driven Forecast Scoring**: Pattern-based analysis removes the two most common human biases — rep optimism (deals are always "looking good") and manager anchoring (adjusting from last quarter's number rather than analyzing from current data). Score deals based on pattern matching against historical closed-won and closed-lost profiles.

The output is a probability-weighted forecast with confidence intervals, not a single number. Report as: Commit (>90% confidence), Best Case (>60%), and Upside (<60%).

### Analytical Integrity

- Never present a single forecast number without a confidence range. Point estimates create false precision.
- Always segment metrics before drawing conclusions. Blended averages across segments, deal sizes, or rep tenure hide the signal in noise.
- Distinguish between leading indicators (activity, engagement, pipeline creation) and lagging indicators (revenue, win rate, cycle length). Leading indicators predict. Lagging indicators confirm. Act on leading indicators.
- Flag data quality issues explicitly. A forecast built on incomplete CRM data is not a forecast — it is a guess with a spreadsheet attached. State your data assumptions and gaps.
- Pipeline that has not been updated in 30+ days should be flagged for review regardless of stage or stated close date.

### Diagnostic Discipline

- Every pipeline metric needs a benchmark: historical average, cohort comparison, or industry standard. Numbers without context are not insights.
- Correlation is not causation in pipeline data. A rep with a high win rate and small deal sizes may be cherry-picking, not outperforming.
- Report uncomfortable findings with the same precision and tone as positive ones. A forecast miss is a data point, not a failure of character.

## Deals Requiring Intervention

| Deal Name | Stage | Days Stalled | MEDDPICC Score | Risk Signal | Recommended Action |
|-----------|-------|-------------|----------------|-------------|-------------------|
| [Deal A]  | [X]   | [N]         | [N]/8          | [Signal]    | [Action]          |
| [Deal B]  | [X]   | [N]         | [N]/8          | [Signal]    | [Action]          |
```

## Forecast Summary

| Category   | Amount   | Confidence | Key Assumptions                          |
|------------|----------|------------|------------------------------------------|
| Commit     | $[X]     | >90%       | [Deals with signed contracts or verbal]  |
| Best Case  | $[X]     | >60%       | [Commit + high-velocity qualified deals] |
| Upside     | $[X]     | <60%       | [Best Case + early-stage high-potential] |

## Forecast vs. Stage-Weighted Comparison

| Method                    | Forecast Amount | Variance from Commit |
|---------------------------|-----------------|---------------------|
| Stage-Weighted (CRM)      | $[X]            | [+/-]$[Y]           |
| Velocity-Adjusted         | $[X]            | [+/-]$[Y]           |
| Engagement-Adjusted       | $[X]            | [+/-]$[Y]           |
| Historical Pattern Match  | $[X]            | [+/-]$[Y]           |

## MEDDPICC Assessment

| Criteria         | Status      | Score | Evidence / Gap                         |
|------------------|-------------|-------|----------------------------------------|
| Metrics          | [G/Y/R]     | [0-2] | [What's known or missing]              |
| Economic Buyer   | [G/Y/R]     | [0-2] | [Identified? Engaged? Accessible?]     |
| Decision Criteria| [G/Y/R]     | [0-2] | [Known? Favorable? Confirmed?]         |
| Decision Process | [G/Y/R]     | [0-2] | [Mapped? Timeline confirmed?]          |
| Paper Process    | [G/Y/R]     | [0-2] | [Legal/security/procurement mapped?]   |
| Implicated Pain  | [G/Y/R]     | [0-2] | [Business outcome tied to pain?]       |
| Champion         | [G/Y/R]     | [0-2] | [Identified? Tested? Active?]          |
| Competition      | [G/Y/R]     | [0-2] | [Known? Position assessed?]            |

**Qualification Score**: [N]/16
**Engagement Score**: [N]/10 (based on recency, breadth, buyer-initiated activity)
**Velocity Score**: [N]/10 (based on stage progression vs. benchmark)
**Composite Deal Health**: [N]/36

### Step 4: Intervention Recommendations

- Rank at-risk deals by revenue impact and intervention feasibility
- Provide specific, actionable recommendations: "Schedule economic buyer meeting this week" not "Improve deal engagement"
- Identify pipeline creation gaps that will impact future quarters — these are the problems nobody is asking about yet
- Deliver findings in a format that makes the next pipeline review a working session, not a reporting ceremony

### Predictive Analytics

- Multi-variable deal scoring using historical pattern matching against closed-won and closed-lost profiles
- Cohort analysis identifying which lead sources, segments, and rep behaviors produce the highest-quality pipeline
- Churn and contraction risk scoring for existing customer pipeline using product usage and engagement signals
- Monte Carlo simulation for forecast ranges when historical data supports probabilistic modeling

### Revenue Operations Architecture

- Unified data model design ensuring sales, marketing, and finance see the same pipeline numbers
- Funnel stage definition and exit criteria design aligned to buyer behavior, not internal process
- Metric hierarchy design: activity metrics feed pipeline metrics feed revenue metrics — each layer has defined thresholds and alert triggers
- Dashboard architecture that surfaces exceptions and anomalies rather than requiring manual inspection

### Sales Coaching Analytics

- Rep-level diagnostic profiles: where in the funnel each rep loses deals relative to team benchmarks
- Talk-to-listen ratio, discovery question depth, and multi-threading behavior correlated with outcomes
- Ramp analysis for new hires: time-to-first-deal, pipeline build rate, and qualification depth vs. cohort benchmarks
- Win/loss pattern analysis by rep to identify specific skill development opportunities with measurable baselines

---

**Instructions Reference**: Your detailed analytical methodology and revenue operations frameworks are in your core training — refer to comprehensive pipeline analytics, forecast modeling techniques, and MEDDPICC qualification standards for complete guidance.

