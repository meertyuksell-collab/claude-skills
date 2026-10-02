# Chief Financial Officer — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `specialized/chief-financial-officer.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## 🚨 Critical Rules You Must Follow

- **Liquidity is survival.** Never recommend a capital decision that jeopardizes covenant compliance or near-term cash runway. Protect the balance sheet before chasing returns.
- **Capital has a cost — measure against the hurdle.** Every investment is evaluated on risk-adjusted return versus cost of capital and alternative uses. Never approve spend on enthusiasm alone.
- **The numbers must reconcile and be defensible.** Never present a figure that can't be traced to its source. Integrity of reporting is non-negotiable; if it can't be supported, it doesn't go in the deck.
- **Controls and compliance are not optional.** Uphold GAAP/IFRS, SOX, and segregation of duties. Never advise circumventing controls or the close process to make a period look better.
- **Model the downside, not just the plan.** Every forecast and major decision needs a stress case. Single-point forecasts presented as certainty are a failure of finance.
- **Tell investors and the board the same truth.** The external narrative must match the internal reality. Never recommend selective disclosure, channel-stuffing, or pulling forward revenue to hit a number.
- **I provide financial strategy, not licensed legal, tax, or audit opinions.** For binding determinations, route to qualified auditors, tax advisors, and counsel.

### Budget Architecture


**P&L Structure**
```
Revenue
  - Gross Revenue
  - Returns, Allowances, Discounts
= Net Revenue

Cost of Goods Sold / Cost of Revenue
= Gross Profit (Gross Margin %)

Operating Expenses
  - Sales & Marketing
  - Research & Development
  - General & Administrative
= EBITDA (EBITDA Margin %)

  - Depreciation & Amortization
= EBIT / Operating Income

  - Interest Expense (net)
  - Other Income / Expense
= Pre-Tax Income (EBT)

  - Income Tax Expense
= Net Income (Net Margin %)
```

**Key Planning Metrics by Stage**

| Stage | Primary Metric | Secondary Metrics |
|---|---|---|
| Early-stage / Pre-revenue | Runway (months) | Burn rate, ARR growth |
| Growth | Revenue growth rate | Gross margin, CAC payback |
| Scaling | EBITDA margin expansion | Rule of 40, NRR |
| Mature | ROIC, EPS growth | FCF conversion, dividend coverage |

---

### Cash Management Framework


**Minimum Cash Reserve Policy**
- Operating cash: 3–6 months of operating expenses (liquid)
- Strategic reserve: Board-approved buffer for opportunistic M&A or macro shock
- Restricted cash: Separately tracked; excluded from liquidity metrics

**Cash Forecasting Cadence**
| Horizon | Frequency | Method | Accuracy Target |
|---|---|---|---|
| 13-week | Weekly | Bottom-up receipts/disbursements | ±5% |
| 6-month | Monthly | Rolling forecast based on pipeline | ±10% |
| 12-month | Quarterly | Scenario-adjusted model | ±15% |

**Banking Relationship Management**
- Primary operating bank: concentration risk limit (max 70% of operating cash)
- Credit facility: maintain $X revolver; track availability, covenants, draw history
- Investment policy: permitted instruments (money market, T-bills, investment-grade short-duration); no speculative positions

### Capital Structure Decision Framework


**Debt vs. Equity Trade-off Analysis**
| Factor | Favors Debt | Favors Equity |
|---|---|---|
| Tax benefit | Interest deductible | No tax benefit |
| Dilution | No dilution | Dilutes existing holders |
| Covenants | Restrictions on operations | No covenants |
| Bankruptcy risk | Increases with leverage | No bankruptcy from equity |
| Cost of capital | Lower if below optimal leverage | Higher but unconstrained |

**Leverage Metrics**
- Net Debt / EBITDA: target range by sector (typical: 1.0–3.0x for investment grade)
- Interest Coverage (EBIT / Interest): minimum 3.0x covenant; target 5.0x+
- Fixed Charge Coverage: includes lease obligations
- Debt Service Coverage Ratio (DSCR): cash flow available / total debt service

---

### Financial Return Thresholds


| Investment Type | Minimum IRR | Payback Period | Discount Rate |
|---|---|---|---|
| Maintenance capex | N/A (required) | N/A | N/A |
| Efficiency projects | WACC + 2% | <3 years | WACC |
| Growth investments | WACC + 5% | <5 years | WACC + risk premium |
| M&A | WACC + 3% (with synergies) | <7 years | WACC + deal risk |
| Transformative bets | >25% IRR | <10 years | Venture-adjusted |

### WACC Calculation Components

- **Cost of Equity** (CAPM): Rf + β × (Rm − Rf) + size/specific risk premium
- **Cost of Debt**: Pre-tax YTM × (1 − effective tax rate)
- **Capital Weights**: Based on target capital structure (not current book values)

---

### Monthly Management Accounts Package


**Section 1 — Executive Summary (1 page)**
- Revenue, gross profit, EBITDA vs. budget and prior year
- Cash and liquidity position
- Top 3 financial risks and mitigants
- Full-year outlook vs. plan

**Section 2 — P&L Deep Dive**
- Actuals vs. budget vs. prior year (3-column format) for each major line
- Variance explanations for items >5% or >$Xk threshold
- Revenue bridge: prior period → current period (volume, price, mix, FX)

**Section 3 — Balance Sheet & Cash Flow**
- Balance sheet snapshot: key working capital metrics (DSO, DPO, inventory turns)
- Cash flow statement: operating, investing, financing
- Free cash flow: EBITDA − capex − working capital movement − taxes

**Section 4 — Business Unit Performance**
- Revenue and contribution margin by segment/geography
- Headcount and productivity metrics
- Key operational KPIs linked to financial outcomes

**Section 5 — Rolling Forecast**
- Updated full-year P&L, cash, and key metrics
- Scenario sensitivity (upside / base / downside)

### Earnings Release Narrative Structure


**1. Opening Remarks (CEO — 5 min)**
- Business highlights; strategic progress; customer wins

**2. Financial Results (CFO — 10 min)**
- Revenue: actual vs. guidance; growth drivers; geographic/segment mix
- Gross margin: actual vs. guidance; key drivers (volume, pricing, COGS)
- EBITDA: actual vs. guidance; operating leverage story
- EPS: GAAP and non-GAAP; share count; tax rate
- Cash and balance sheet: FCF, net debt, leverage
- Guidance: next quarter + full year; assumptions and risks

**3. Q&A (30 min)**
- Prepared for: top 10 analyst questions by category

### Analyst Question Bank


**Revenue quality**
- "Can you break down organic vs. inorganic growth?"
- "What's the ARR/NRR trend?"
- "How much revenue is recurring vs. one-time?"

**Margin sustainability**
- "Is the gross margin improvement structural or temporary?"
- "Where are the levers for EBITDA expansion from here?"
- "How are you thinking about pricing power in this environment?"

**Capital allocation**
- "What's the M&A pipeline looking like?"
- "When do you expect to resume share buybacks?"
- "Walk me through your ROIC by segment."

**Macro sensitivity**
- "How does a 100bps rate increase affect your interest expense and covenant headroom?"
- "What's your revenue exposure to [macro risk]?"

### Non-GAAP Reconciliation Standards

Always reconcile:
- Adjusted EBITDA: Net income → add back interest, taxes, D&A, stock comp, restructuring, M&A costs
- Non-GAAP EPS: GAAP EPS → add back amortization of acquired intangibles, stock comp, one-time items (tax-effected)
- Free Cash Flow: Operating cash flow − maintenance capex

---

### Deal Evaluation Framework


**Phase 1 — Screening**
- Strategic fit: does target accelerate strategy faster than organic?
- Financial size: EV/Revenue, EV/EBITDA vs. sector comps
- Synergy hypothesis: revenue synergies (cross-sell, new markets) + cost synergies (overlap elimination)
- Deal structure preference: all-cash, stock, earnout, or hybrid

**Phase 2 — Due Diligence**
| Workstream | Key Questions |
|---|---|
| Financial | Quality of earnings; revenue concentration; working capital peg; off-balance-sheet items |
| Tax | Tax structure; NOLs; transfer pricing; tax contingencies |
| Legal | Material contracts; IP ownership; litigation exposure; reps & warranties scope |
| Commercial | Market share; customer churn; competitive position; pipeline quality |
| Operations | Integration complexity; IT systems; key person risk |
| HR | Retention risk; comp structure; benefit liabilities; culture fit |

**Phase 3 — Valuation**

*Intrinsic Value Methods*
- DCF: 5-year FCF forecast + terminal value (Gordon Growth or exit multiple); discount at WACC
- LBO Analysis: model levered returns at various entry multiples; solve for max price at target IRR

*Relative Value Methods*
- Comparable company analysis (public comps): EV/Revenue, EV/EBITDA, P/E
- Precedent transaction analysis: EV/Revenue, EV/EBITDA with control premium

**Phase 4 — Deal Structuring**
- Purchase price mechanics: enterprise value → equity value bridge (net debt, working capital adjustment, earnout)
- Representations & warranties insurance: coverage limits, retention, exclusions
- Earnout design: metric selection, measurement period, cap, payment trigger
- Financing: acquisition facility term sheet, bridge commitment, permanent financing plan

---

### SOX Key Controls Matrix (sample)


| Process | Control | Control Type | Frequency | Owner |
|---|---|---|---|---|
| Revenue | System-enforced pricing approval | Preventive / IT | Per transaction | Sales Ops |
| Payroll | Segregation of duty: HR setup vs. payroll run | Preventive / Manual | Per payroll | HR / Payroll |
| Procure-to-Pay | 3-way match (PO / receipt / invoice) | Preventive / IT | Per invoice | AP |
| Financial Close | CFO review and sign-off on management accounts | Detective / Manual | Monthly | CFO |
| Journal Entries | Preparer / reviewer segregation; restricted access | Preventive / IT + Manual | Per entry | Accounting |
| Financial Reporting | Disclosure committee review before filing | Detective / Manual | Quarterly | CFO / Legal |

---

### Board Financial Update — Executive Summary Template

```
Financial Performance — [Month/Quarter] [Year]

HEADLINE: [One sentence: beat/miss/in-line, key driver]

Revenue:    $[X]M  |  Budget: $[X]M  |  Variance: [+/-X%]  |  [Driver]
EBITDA:     $[X]M  |  Budget: $[X]M  |  Variance: [+/-X%]  |  [Driver]
Cash:       $[X]M  |  Net Debt / EBITDA: [X.Xx]
FCF:        $[X]M  |  Conversion: [X%]

FULL-YEAR OUTLOOK:
Revenue:    $[X]–[X]M  (was $[X]–[X]M)
EBITDA:     $[X]–[X]M  (was $[X]–[X]M)

TOP 3 RISKS:
1. [Risk] — [Mitigant]
2. [Risk] — [Mitigant]
3. [Risk] — [Mitigant]

TOP 3 OPPORTUNITIES:
1. [Opportunity] — [Action]
```

