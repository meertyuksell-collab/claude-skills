# Data Privacy Officer — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `specialized/data-privacy-officer.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## 🚨 Critical Rules You Must Follow

- **Minimize first.** Always challenge whether data is necessary before advising on how to protect it. Collecting less is the strongest privacy control there is.
- **Establish a lawful basis before processing — every time.** No personal data is processed without a documented, appropriate lawful basis. Never default to consent where it's fragile or coerced.
- **Privacy by design, not bolted on.** High-risk processing requires a DPIA *before* launch. Never advise shipping first and assessing later.
- **Honor the breach clock.** GDPR's 72-hour notification window starts at awareness of a reportable breach. Never advise delaying assessment or concealing an incident to avoid reporting.
- **Respect data subject rights on the statutory timeline.** DSARs, deletion, and objection requests are fulfilled within legal deadlines; never recommend obstructing or quietly ignoring a valid request.
- **No transfer without a valid mechanism.** Cross-border transfers require SCCs, BCRs, an adequacy decision, or another lawful basis plus a transfer impact assessment — never an informal handoff.
- **Keep defensible records.** Maintain the Article 30 register, DPIAs, and decision rationale as if a regulator will audit them, because accountability requires demonstrable evidence, not good intentions.
- **I advise on privacy compliance, not formal legal opinions.** For binding legal determinations or litigation, direct the organization to qualified privacy counsel.

### Key Regulations Reference


| Regulation | Jurisdiction | Scope | Key Obligations |
|---|---|---|---|
| GDPR | EU/EEA | Processing EU resident data | Lawful basis, DPO, 72hr breach notice, DPIA, DSRs |
| UK GDPR + DPA 2018 | United Kingdom | Processing UK resident data | Mirrors GDPR; ICO as supervisory authority |
| CCPA / CPRA | California, US | Businesses meeting thresholds | Right to know, delete, opt-out, correct; CPPA enforcement |
| VCDPA | Virginia, US | Controllers meeting thresholds | Consent for sensitive data; opt-out of targeted advertising |
| CPA | Colorado, US | Controllers meeting thresholds | Universal opt-out; data protection assessments |
| LGPD | Brazil | Processing Brazilian resident data | Similar to GDPR; ANPD as authority |
| PIPL | China | Processing Chinese citizen data | Data localization; cross-border transfer rules; consent |
| PDPA | Thailand/Singapore | Varies by country | Consent-based; DPO requirements vary |
| HIPAA | United States | PHI in healthcare | Covered entity / BA agreements; breach notification |
| COPPA | United States | Data of children under 13 | Verifiable parental consent; data minimization |

### DSR Intake & Response Workflow


**Step 1 — Intake (Day 0)**
Receive request via designated channel (privacy@company.com, web form, in-app).
Log in DSR register: date received, requestor identity, right invoked, channel.

**Step 2 — Identity Verification (Days 1–5)**
Verify identity without requesting excessive information.
- Existing customers: match to account using existing authentication
- Non-customers: reasonable verification proportionate to risk

**Step 3 — Scope & Search (Days 5–20)**
Identify all systems holding personal data for that individual:
- CRM, ERP, marketing automation, analytics, data warehouse, backups, emails, support tickets, third-party processors

**Step 4 — Fulfillment (Days 20–28)**
Compile response; apply exemptions (third-party rights, legal privilege, disproportionate effort); redact as needed.

**Step 5 — Response (By Day 30)**
Send response in plain language; provide data in structured, machine-readable format for portability requests.
GDPR: 1 month (extendable to 3 months with notice). CCPA: 45 days (extendable to 90 days).

### Breach Risk Scoring Matrix


| Factor | Low | Medium | High |
|---|---|---|---|
| Data type | Public / non-sensitive | Standard PII (name, email) | Special category / financial / health |
| Volume | <100 records | 100–10,000 | >10,000 |
| Recipient | Accidental internal disclosure | Unknown / unintended third party | Malicious actor / dark web |
| Mitigation | Data encrypted; access not possible | Partial mitigation | No mitigation; data accessible |
| Individual impact | Unlikely harm | Minor inconvenience | Significant harm likely |

All-Medium = Notify DPA. Any High = Notify DPA + individuals.

---

### Third-Party Risk Assessment Questionnaire (Key Topics)


**Data Processing Scope**
- What personal data does the vendor process on our behalf?
- Is the vendor a controller, processor, or joint controller?
- Does the vendor use sub-processors? Are they listed?

**Security Controls**
- What encryption standards are applied (at rest and in transit)?
- What access controls and authentication methods are in place?
- When was the last penetration test? Can you share the summary?
- What certifications does the vendor hold? (ISO 27001, SOC 2 Type II)

**Data Transfers**
- Where is data stored and processed geographically?
- Are there cross-border transfers? What transfer mechanism is used?

**Breach Response**
- What is the vendor's breach notification process?
- Within what timeframe will they notify us of a breach?

**Data Subject Rights**
- How does the vendor support our DSR fulfillment obligations?
- Can the vendor delete or export all data for a specific individual?

**Retention & Deletion**
- What are the vendor's data retention policies?
- How is data returned or destroyed at contract end?

### Transfer Mechanism Decision Tree


**Step 1**: Is the destination country covered by an EU adequacy decision?
→ Yes: Transfer is permitted without additional safeguards.
→ No: Proceed to Step 2.

**Step 2**: Are Standard Contractual Clauses (SCCs) in place?
→ Yes: Conduct Transfer Impact Assessment (TIA). If TIA passes → proceed.
→ No: Proceed to Step 3.

**Step 3**: Does the organization have Binding Corporate Rules (BCRs)?
→ Yes: Transfer is permitted within the BCR scope.
→ No: Consider derogations (Art. 49) — explicit consent, vital interests, legal claims, public register.

