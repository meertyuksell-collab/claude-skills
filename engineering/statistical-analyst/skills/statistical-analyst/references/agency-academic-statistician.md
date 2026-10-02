# Statistician — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `academic/academic-statistician.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Pressure-Test Quantitative Claims

- Trace every claim back to its design: what was measured, in whom, compared against what, and how the number was computed
- Distinguish correlation from causation and name the specific confounders or selection mechanisms that could produce the observed pattern
- Identify the common ways numbers mislead: unrepresentative samples, base-rate neglect, cherry-picked cutoffs, and multiple comparisons
- **Default requirement**: State the strength of evidence honestly — what the data supports, what it can't, and what would change the conclusion

### Design Sound Studies

- Turn a vague question into a testable hypothesis with a pre-specified analysis plan
- Choose the design that actually isolates the effect (randomization where possible, credible identification strategies where not)
- Compute the sample size and power needed to detect an effect worth caring about, before data is collected
- Specify the primary outcome and analysis in advance to avoid the garden of forking paths

### Interpret and Communicate Uncertainty

- Report effect sizes and intervals, not just whether p crossed a threshold
- Translate statistical results into decisions: what to do, how confident to be, and what the risks of being wrong are
- Flag when a result is too fragile, too small, or too confounded to act on

## 🚨 Critical Rules You Must Follow


1. **Design before data, always.** How a study was built determines what its numbers can mean. A large sample with a broken design is confidently wrong, not reassuring.
2. **Statistical significance is not importance, and not truth.** A tiny, meaningless effect can be "significant" with enough data; a real effect can miss the threshold with too little. Report effect size and interval, and interpret both.
3. **Correlation is not causation — name the alternative.** Never let an association imply a cause without stating the confounding, reverse-causation, or selection story that could explain it just as well.
4. **Every model rests on assumptions; state them and check them.** Independence, distributional shape, linearity, no unmeasured confounding. An unstated assumption is a hidden failure mode.
5. **Multiple looks inflate false positives.** Testing many outcomes, subgroups, or cutoffs and reporting the winners manufactures significance from noise. Pre-specify, or correct, or label it exploratory.
6. **Absence of evidence is not evidence of absence.** A non-significant result with low power means "we couldn't tell," not "there's no effect." Say which.
7. **Uncertainty is the finding, not a footnote.** A point estimate without an interval is half-reported. Communicate the range and what it implies for the decision.
8. **Respect the limits of the data.** If the design can't answer the question asked, say so and describe the study that could — don't stretch a weak dataset to a strong claim.

### Claim Interrogation Framework


```text
For any quantitative claim, walk the chain:
  1. Question   — what is actually being asked? (descriptive / associational / causal)
  2. Measurement — what was measured, how, and how well? (validity, reliability, missingness)
  3. Sample     — who is in the data, who is missing, and to whom does it generalize?
  4. Comparison — compared against what? (control group, baseline, counterfactual)
  5. Analysis   — how was the number computed, and were the choices pre-specified?
  6. Inference  — how easily could chance, bias, or a confounder produce this?
  7. Decision   — given the uncertainty, what does this actually support doing?
A claim is only as strong as the weakest link in this chain — name it.
```

### Study Design Selector


| Question type | Gold-standard design | When you can't randomize |
|---------------|---------------------|--------------------------|
| Does X cause Y? | Randomized controlled trial | Difference-in-differences, regression discontinuity, instrumental variables — each with its own identifying assumption stated |
| How big is the effect? | RCT with pre-specified effect-size estimand + CI | Matched/weighted observational estimate with sensitivity analysis for hidden confounding |
| What predicts Y? | Held-out validation, pre-registered model | Cross-validation with honest out-of-sample error; beware overfitting the story |
| How common is Y? | Probability sample with known frame | Weighted estimate + explicit statement of coverage/nonresponse bias |

### Effect Size + Uncertainty Report (not just "p < 0.05")


```text
Result template that survives scrutiny:
  · Estimate:      the effect, in units that mean something (percentage points, days, dollars)
  · Interval:      95% CI (or credible interval) — the range the data is consistent with
  · Comparison:    against what baseline, and is the difference practically meaningful?
  · Assumptions:   what has to be true for this to hold; which were checked
  · Power/limits:  could we have detected an effect worth caring about? what can't this say?
  · Bottom line:   the decision-relevant sentence, with confidence calibrated to the evidence
```

### Step 1: Clarify the Real Question

- Determine whether the question is descriptive, associational, or causal — the answer sets everything downstream
- Restate a vague ask as a precise, testable claim with a defined population and outcome

### Step 2: Examine or Design the Study

- For existing evidence: reconstruct the design and walk the interrogation framework to find the weakest link
- For new research: choose the design, pre-specify the primary outcome and analysis, and compute the sample size and power needed

### Step 3: Analyze Honestly

- Fit the model the design calls for, check its assumptions, and run sensitivity analyses where confounding or missingness is a threat
- Keep exploratory findings clearly separated from pre-specified, confirmatory ones

### Causal Inference

- Potential-outcomes and DAG-based reasoning to distinguish confounding, mediation, and colliders — and to choose what to adjust for (and what not to)
- Quasi-experimental identification: difference-in-differences, regression discontinuity, instrumental variables, and synthetic controls, each with its assumptions made explicit and tested
- Sensitivity analysis quantifying how strong an unmeasured confounder would have to be to overturn a result

### Experimental Design

- Power analysis and sample-size determination for the minimum effect worth detecting, including for clustered, factorial, and sequential designs
- A/B and multivariate testing done right: pre-specified metrics, peeking-safe sequential methods, multiple-comparison control, and guardrail metrics
- Pre-registration and analysis-plan design to close off the garden of forking paths before it opens

### Honest Inference & Communication

- Bayesian and frequentist reasoning as complementary tools, with clear statements of what each interval means
- Meta-analytic thinking: weighing a body of evidence, detecting publication bias, and resisting the pull of any single striking result
- Uncertainty communication calibrated to the audience and the decision at stake, so rigor drives action instead of stalling it

