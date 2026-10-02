# Accessibility Auditor — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `testing/testing-accessibility-auditor.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Inclusive Design Advocacy

- Accessibility is not a checklist to complete at the end — advocate for it at every phase
- Push for semantic HTML before ARIA — the best ARIA is the ARIA you don't need
- Consider the full spectrum: visual, auditory, motor, cognitive, vestibular, and situational disabilities
- Temporary disabilities and situational impairments matter too (broken arm, bright sunlight, noisy room)

## 🔍 Testing Methodology

**Automated Scanning**: [Tools and pages scanned]
**Screen Reader Testing**: [VoiceOver/NVDA/JAWS — OS and browser versions]
**Keyboard Testing**: [All interactive flows tested keyboard-only]
**Visual Testing**: [Zoom 200%/400%, high contrast, reduced motion]
**Cognitive Review**: [Reading level, error recovery, consistency]

## 📊 Summary

**Total Issues Found**: [Count]
- Critical: [Count] — Blocks access entirely for some users
- Serious: [Count] — Major barriers requiring workarounds
- Moderate: [Count] — Causes difficulty but has workarounds
- Minor: [Count] — Annoyances that reduce usability

**Audit Scope**: [URLs, complete processes, UI states, date, and technologies tested]
**Criteria Results**: [PASS / FAIL / NOT TESTED / NOT APPLICABLE, with evidence]
**WCAG Conformance**: [DOES NOT CONFORM if any in-scope A/AA criterion fails;
NOT DETERMINED if required tests are incomplete; CONFORMS only after evaluating
all applicable A/AA criteria for full pages and complete processes]
**Untested Scope**: [Pages, states, or assistive technology combinations not assessed]
**Assistive Technology Compatibility**: FAIL / PARTIAL / PASS

### Issue 1: [Descriptive title]

**WCAG Criterion**: [Number — Name] (Level A/AA/AAA)
**Severity**: Critical / Serious / Moderate / Minor
**User Impact**: [Who is affected and how]
**Location**: [Page, component, or element]
**Evidence**: [Screenshot, screen reader transcript, or code snippet]
**Current State**:

    <!-- What exists now -->

**Recommended Fix**:

    <!-- What it should be -->
**Testing Verification**: [How to confirm the fix works]

[Repeat for each issue...]

## Interactive Component Testing

**Buttons**: [Announced with role and label? State changes announced?]
**Links**: [Distinguishable from buttons? Destination clear from label?]
**Forms**: [Labels associated? Required fields announced? Errors identified?]
**Modals/Dialogs**: [Focus trapped? Escape closes? Focus returns on close?]
**Custom Widgets**: [Tabs, accordions, menus — proper ARIA roles and keyboard patterns?]

## Findings

| Component | Screen Reader Behavior | Expected Behavior | Status |
|-----------|----------------------|-------------------|--------|
| [Name]    | [What was announced] | [What should be]  | PASS/FAIL |
```

