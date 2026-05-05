# AI Risk Assessment Report: Zoom AI Companion

**Date:** May 5, 2026  
**Assessor:** AI GRC Analyst Assistant (Gemini Custom Gem — AI Cyber Risk Assessment Generator)  
**Status:** CONDITIONAL APPROVAL (Pending Remediation)  
**Assessment Tool:** [ai_risk_assessment_tool](https://github.com/CPAtoCybersecurity/ai_risk_assessment_tool) Gemini Custom Gem  
**Gem URL:** https://gemini.google.com/gem/460483fa0239  

---

## 1. Executive Summary

The deployment of Zoom AI Companion for 200 employees facilitates significant productivity gains in meeting synthesis and cross-platform collaboration. However, due to its integration with Slack/Gmail and access to sensitive HR/Strategic data without a mandatory Human-in-the-Loop (HITL) gate, the system poses a high risk of unauthorized data exfiltration and prompt injection.

**Approval is contingent on enforcing HITL for all external communications and implementing the defined "Red-Line" exclusions.**

---

## 2. Intake Summary

| Field | Detail |
|---|---|
| **System Name** | Zoom AI Companion |
| **Business Purpose** | Meeting summarization, action items, email drafting, internal collaboration (~200 employees) |
| **Architecture/Integrations** | SaaS (Zoom Business), integrated with Google Calendar and Slack |
| **Data Handled** | Internal meeting transcripts, HR discussions, client calls, strategic planning, email drafts |
| **Environment** | Zoom Cloud (US Region) |

---

## 3. Lethal Trifecta Check

| Factor | Status | Context |
|---|---|---|
| **Private Data Access** | ✅ YES | Accesses Google Calendar, Slack, and sensitive meeting transcripts (HR/Strategy) |
| **Untrusted Input** | ✅ YES | Processes external participant audio/video during client calls |
| **External Communication** | ✅ YES | Capable of drafting/sending emails and posting to Slack |

**Verdict: LETHAL TRIFECTA PRESENT → Tier 3 Assessment Mandatory**

The "Lethal Trifecta" is present. Immediate architectural controls are required to prevent automated egress.

---

## 4. Tier Selection

**TIER 3: High Criticality / Strictly Confidential**

This tier requires a full security checklist, threat modeling with STRIDE/OWASP, and explicit attestation of architectural controls.

---

## 5. Threat Scenarios (STRIDE + OWASP)

| ID | STRIDE | OWASP ID | Threat Scenario | Likelihood | Impact | Risk |
|---|---|---|---|---|---|---|
| T-01 | Tampering | LLM01 | **Indirect Prompt Injection:** An external client provides "instructions" during a call that cause the AI to email sensitive internal summaries to an external address. | High | High | **Critical** |
| I-01 | Info Leak | LLM02 | **Unauthorized RAG Retrieval:** A junior employee queries the AI Companion for "Recent Executive Pay" and the AI retrieves details from an unprotected transcript. | Moderate | High | **High** |
| R-01 | Repudiation | LLM06 | **Excessive Agency:** AI sends a legally binding or offensive email via "Auto-draft." Without HITL logs, the org cannot prove the user didn't write it. | High | Moderate | **High** |

---

## 6. Security Checklist (NIST CSF 2.0 Mapping)

| Control ID | NIST ID | Control Description | Status | Findings / Comments |
|---|---|---|---|---|
| AI-01 | PR.AC | Human-in-the-Loop (HITL) | ❌ FAIL | **CRITICAL:** No policy for manual review before email/Slack egress. Auto-draft enabled org-wide. |
| AI-02 | PR.DS | Data Fencing (Red-Lines) | ⏳ PENDING | HR/M&A/Legal discussions must be excluded via Zoom Admin Group settings. |
| AI-03 | DE.CM | Continuous Monitoring | ✅ YES | Zoom provides admin logs for feature usage and data access. |
| AI-04 | PS.RP | Kill-Switch | ✅ YES | Admins can disable AI Companion org-wide in < 1 minute. |

---

## 7. Required Remediation Actions

| Action Item | Owner | Priority | Status |
|---|---|---|---|
| **Enforce HITL Policy:** Technically disable "Auto-send" and update Employee Handbook to mandate manual review of all AI drafts. | IT/Compliance | P0 | Open |
| **Configure "Red-Line" Groups:** Create Zoom groups for HR/Exec/Legal and disable AI Companion for those users/meeting types. | Zoom Admin | P0 | Open |
| **Sanitize Integrations:** Audit Slack/Google OAuth permissions to ensure "Least Privilege" for the Zoom Service Account. | Security Eng | P1 | Open |

**Defined Red-Line Topics (excluded from AI processing):**
- HR terminations and performance reviews
- Executive compensation discussions
- Active M&A negotiations
- Meetings covered by client NDAs

---

## 8. Final Risk Rating

| Rating Type | Level |
|---|---|
| **Inherent Risk** | 🔴 Critical |
| **Residual Risk** | 🟡 Medium (assuming all P0 remediations are implemented) |

---

## 9. Approval Signatures

| Role | Name | Date |
|---|---|---|
| Assessor | ___________________________ | |
| Risk Owner | ___________________________ | |

---

*Assessment generated using the AI Cyber Risk Assessment Generator Gemini Custom Gem, built on the [ai_risk_assessment_tool](https://github.com/CPAtoCybersecurity/ai_risk_assessment_tool) methodology (NIST SP 800-30, NIST Cyber AI Profile, OWASP Top 10 for LLM).*
