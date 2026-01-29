# Risk Assessment Output Template

## Document Structure

```markdown
# [System Name] Risk Assessment

**Date:** YYYY-MM-DD | **Assessor:** [Name] | **Tier:** 1/2/3

## Executive Summary

[2-3 sentences: system, findings, risk level, recommendation]

**Overall Risk:** Low / Medium / High / Critical
**Recommendation:** Approve / Approve with Conditions / Reject

---

## Lethal Trifecta

| Factor | Present | Details |
|--------|:-------:|---------|
| Private data access | Y/N | |
| Untrusted input | Y/N | |
| External communication | Y/N | |

**Status:** Clear / Elevated / High / CRITICAL

---

## System Overview

**Objective:** [Why it exists]
**Users:** [Who uses it]
**Components:** [LLM, agents, tools, etc.]

---

## Threat Scenarios

| ID | Threat | L | I | Risk | STRIDE |
|----|--------|:-:|:-:|:----:|:------:|
| T1 | | | | | |
| T2 | | | | | |
| T3 | | | | | |

L=Likelihood, I=Impact (VL/L/M/H/VH)

**Residual Risk:** Low / Medium / High / Critical

---

## Remediation

| # | Action | Priority | Owner |
|---|--------|:--------:|-------|
| 1 | | Crit/High/Med | |
| 2 | | | |

---

## Controls Summary

| Area | Status |
|------|:------:|
| Authentication | ✓/✗/N/A |
| Authorization | |
| Encryption | |
| Input validation | |
| Prompt injection defense | |
| Tool scoping | |
| Human-in-the-loop | |
| Logging | |
| Kill switch | |

---

## Approval

| Role | Name | Decision | Date |
|------|------|----------|------|
| Assessor | | Complete | |
| Security | | Approve/Reject | |
| Risk Owner | | Accept Risk | |

---

## Appendix

**Assumptions:** [List]
**Open Questions:** [List]
```

## Tier Adjustments

**Tier 1:** Executive Summary + Trifecta + Top 5 threats + Go/No-Go only

**Tier 2:** Full template

**Tier 3:** Full template + attack path narratives + data flow diagrams + compliance mapping
