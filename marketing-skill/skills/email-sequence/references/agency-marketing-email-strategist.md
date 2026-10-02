# Email Marketing Strategist — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `marketing/marketing-email-strategist.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Segmentation Over Broadcast

Every campaign targets a specific segment defined by at least two attributes (e.g., language + lifecycle stage, or transaction type + engagement recency). Single-attribute segments are acceptable only for basic reporting.

### Respect the Lifecycle

A Won client never receives a cold nurture email. A Lost lead never receives a review request. A contact marked Irrelevant never enters any sequence. Email strategy reflects where contacts ARE now, not where they were at capture.

### Clicks Over Opens

Post-Apple MPP (40-60% of most lists use Apple Mail), open rates are inflated and unreliable. CTR, CTOR, and conversion rate are the real performance indicators. Never use open rate as the sole success metric. Average 2025 open rate was 43.46% across industries -- but this number is meaningless for optimization.

### Exit Conditions Are Non-Negotiable

Every automated sequence defines explicit exit conditions: conversion achieved, unsubscribe received, hard bounce detected, complaint filed, inactivity threshold reached, duplicate detected. No sequence runs indefinitely.

### Data Quality Before Volume

One bad email (phone concatenated in email field, invalid domain) can crash an entire batch. Validate at capture (regex + MX check for bulk imports). Remove hard bounces immediately. Run quarterly list verification. Clean data = clean reputation.

### Consent Is Infrastructure

Consent is not a checkbox -- it's documented (date, method, source, scope), withdrawable (one-click), and auditable (GDPR Article 7). Never assume consent from a static list import. Double opt-in is the safest approach even though it's not legally mandatory in all jurisdictions.

## CRM → ESP Attribute Map


| CRM Field | ESP Attribute | Type | Values | Sync |
|-----------|--------------|------|--------|------|
| Lang | LANGUAGE | category | EN=1, BG=2, FR=3 | Zapier (capture) + n8n (update) |
| Status | LEAD_STATUS | category | Lost=1, Gave Up=2, Active=3, Won=4, 1st Contact=5 | n8n (on status change) |
| Transaction | TRANSACTION | category | Buy=1, Sell=2, Rent=3, Rent Out=4, Other=5 | n8n (when agent updates) |
| Name | FIRSTNAME | text | Free text | Zapier (capture) |

Notes:
- Category attributes require numeric IDs, not text values
- Empty/null: skip attribute in upsert, don't overwrite with empty
- Case-sensitive in most ESPs
```

### Email-Level Metrics

| Metric | Good | Great | Alert |
|--------|------|-------|-------|
| CTR (overall) | > 2% | > 5% | < 1% |
| CTR (property alerts) | > 10% | > 15% | < 5% |
| CTOR | > 10% | > 20% | < 5% |
| Conversion rate (alert → inquiry) | > 3% | > 8% | < 1% |
| Conversion rate (nurture → inquiry) | > 0.5% | > 2% | < 0.2% |
| Unsubscribe rate | < 0.3% | < 0.1% | > 0.5% |
| Complaint rate | < 0.05% | < 0.02% | > 0.10% |
| Hard bounce rate | < 0.5% | < 0.2% | > 1% |

### System-Level Metrics

| Metric | Target |
|--------|--------|
| List growth rate | +2-5% monthly (net) |
| Segment coverage | 100% of active contacts in at least one dynamic segment |
| Automation coverage | 100% of lifecycle stages have an active sequence |
| Deliverability score | > 95% inbox placement |
| CRM-ESP sync lag | < 4 hours for batch, < 5 seconds for event-driven |

### Revenue Metrics

| Metric | Description |
|--------|-------------|
| Revenue per email sent | Total attributed revenue / emails sent |
| Email-sourced pipeline | Leads entered pipeline via email CTA |
| Referral conversion rate | Referred contacts who became clients |
| Review acquisition rate | Review requests that resulted in published reviews |

### AI-Powered Optimization (2025-2026 Production-Ready)


**Send-Time Optimization (STO)**: AI predicts each contact's optimal engagement window based on historical click patterns. Measured lift: 15-23% higher open rates. Critical: modern STO must analyze clicks and conversions, not opens (Apple MPP spoofs opens). Requires 30+ days of engagement data per contact. Available natively in Brevo from Standard plan.

**Subject Line AI**: Generate 3-5 variants, A/B test on 10-20% sample, auto-deploy winner. eBay case study: 15.8% open rate lift, 31% increase in clicks. 64% of email marketers now use AI in their programs; AI personalization drives 41% average revenue increase.

**Brevo Aura AI** (launched May 2025): Chat-style assistant in dashboard and email editor. Generates subject lines, body copy, CTAs, tone adjustments, multilingual translations. Available on free plan.

**Generative Review Suggestions**: Use LLMs (Claude Haiku) to generate personalized Google Review suggestions based on transaction type, language, and client name. Inject via template params ({{ params.SUGGESTED_REVIEW }}). Include in review request emails as copy-paste inspiration.

### Behavioral Trigger Architecture

```
[Property page viewed, no inquiry] → 24h delay → Abandoned browse email
[Form partially filled] → 4h delay → "Finish your inquiry" reminder
[CRM status → Won] → 7-day delay → Review request sequence
[CRM status → Lost, 90+ days] → Reactivation sequence
[Email clicked, no conversion] → 48h delay → Related content follow-up
[3+ property views same city] → Immediate → City-specific property digest
[Client anniversary] → Annual → "Thank you" + referral ask
```

### Real Estate Vertical Playbook

- **Property storytelling** in emails: narrative descriptions that help buyers envision their life there (highest engagement, most underutilized)
- **Market data emails**: price trends by neighborhood, homes sold this week, timing insights (establishes authority)
- **Optimal email length**: 200-300 words for real estate (tested). Shorter = higher CTR. Longer = perceived as newsletter.
- **Best days**: Tuesday and Friday (highest open + CTR across real estate studies)
- **Review request timing**: agent calls client within 7 days of closing. Email follows only after the personal touch. Include direct Google Review link + AI-generated suggested review text.
- **Referral program**: 60-90 days post-closing. Reward structure (cash, service credit, or recognition). Unique tracking per client. Quarterly "thinking of you" to keep referral pipeline warm.

### Post-February 2024 Deliverability Landscape

- **Google** (Feb 2024 + Nov 2025 escalation): SPF + DKIM + DMARC required. One-click unsubscribe required for bulk (5K+/day). Complaint rate < 0.30%. Non-compliant emails now face permanent rejections, not just spam folder.
- **Yahoo**: Aligned with Google requirements (Feb 2024).
- **Microsoft** (May 2025): Enforcing similar standards for Outlook/Hotmail.
- **BIMI**: Display your logo in inbox. Requires DMARC p=quarantine or p=reject + VMC certificate. Worth implementing for brand recognition in competitive verticals.

### GDPR & ePrivacy Compliance (2026 State)

- ePrivacy Regulation withdrawn by European Commission (Feb 2025). Original ePrivacy Directive still applies with member-state variations.
- CNIL draft (June 2025): tracking pixel deployment may require separate consent from marketing email consent. Monitor enforcement.
- GDPR fines increasing: CNIL fined Google 325M EUR (Sept 2025).
- Consent records: store date, time, method, source URL, IP, scope. Not just a checkbox.
- Data retention: document policy. Delete/anonymize after 12-24 months of zero engagement.

