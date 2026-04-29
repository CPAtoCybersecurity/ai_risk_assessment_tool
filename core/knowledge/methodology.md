# AI Risk Assessment Methodology

## Overview

This methodology provides a structured approach for assessing AI and agentic systems before production approval. It follows NIST SP 800-30 principles adapted for AI-specific risks, incorporating the "Lethal Trifecta" early-gate analysis and OWASP guidance for LLM/agentic applications.

---

## Risk Assessment Framework (NIST 800-30)

Risk assessment is a key component of organizational risk management. The process identifies, estimates, and prioritizes information security risks by analyzing:

1. **Threats** to the organization
2. **Vulnerabilities** internal and external to systems
3. **Impact** (harm) that may occur if threats exploit vulnerabilities
4. **Likelihood** that harm will occur

**Risk = Likelihood × Impact**

---

## Assessment Process

### Step 1: System Characterization

Document the system's:
- **Boundaries** - What's in scope, what's out
- **Components** - LLM, agents, tools, databases, APIs
- **Data flows** - How information moves through the system
- **Users** - Who interacts with the system and how
- **Integrations** - External systems, APIs, data sources

### Step 2: Asset Identification

For each asset, determine:
- **Data classification**: Public → Internal → Confidential → Strictly Confidential
- **Critical functions**: What must work for the system to fulfill its purpose
- **Dependencies**: What the system relies on

### Step 3: Lethal Trifecta Analysis (MANDATORY)

The Lethal Trifecta identifies AI systems with elevated risk profiles. **All three factors present = mandatory elevated tier assessment.**

| Factor | Question | Examples |
|--------|----------|----------|
| **Private data access** | Can the AI access sensitive data? | Databases, emails, APIs, files, credentials, PII, financial data |
| **Untrusted input** | Can external parties influence the AI? | Webhooks, emails, Slack messages, public APIs, user-generated content, RAG from external sources |
| **External communication** | Can the AI send data outside? | HTTP requests, posting to GitHub/Slack, sending emails, API calls to third parties |

**Trifecta Decision Matrix:**

| Private Data | Untrusted Input | External Comm | Risk Level |
|--------------|-----------------|---------------|------------|
| No | No | No | Standard |
| Any one | - | - | Elevated attention |
| Any two | - | - | High scrutiny |
| Yes | Yes | Yes | **Critical - Tier 3 required** |

### Step 4: Threat Identification

Apply STRIDE analysis to each component:
- **S**poofing - Can identity be faked?
- **T**ampering - Can data/code be modified?
- **R**epudiation - Can actions be denied?
- **I**nformation Disclosure - Can data leak?
- **D**enial of Service - Can availability be disrupted?
- **E**levation of Privilege - Can access be escalated?

Cross-reference with OWASP Top 10 for LLM Applications and OWASP Top 10 for Agentic Applications.

### Step 5: Vulnerability Analysis

Identify weaknesses that threats could exploit:
- Missing or weak controls
- Configuration errors
- Design flaws
- AI-specific vulnerabilities (prompt injection, data poisoning, excessive agency)

### Step 6: Risk Rating

Combine likelihood and impact to determine risk level.

**Likelihood Scale:**

| Level | Description | Criteria |
|-------|-------------|----------|
| Very Low | Highly unlikely | Strong controls, no known threat activity |
| Low | Unlikely | Good controls, limited threat activity |
| Moderate | Possible | Some controls, active threat landscape |
| High | Likely | Weak controls, known threat activity |
| Very High | Almost certain | No controls, active exploitation |

**Impact Scale:**

| Level | Description | Business Impact |
|-------|-------------|-----------------|
| Very Low | Negligible | Minor inconvenience |
| Low | Limited | Limited damage, easily recoverable |
| Moderate | Serious | Significant damage, recovery possible |
| High | Severe | Major damage, difficult recovery |
| Very High | Catastrophic | Existential threat, regulatory action |

**Risk Matrix:**

| | Very Low Impact | Low | Moderate | High | Very High |
|---|---|---|---|---|---|
| **Very High Likelihood** | Low | Moderate | High | Critical | Critical |
| **High** | Low | Moderate | High | High | Critical |
| **Moderate** | Low | Low | Moderate | High | High |
| **Low** | Low | Low | Low | Moderate | High |
| **Very Low** | Low | Low | Low | Low | Moderate |

### Step 7: Control Recommendations

Prioritize controls by:
1. Risk reduction effectiveness
2. Implementation feasibility
3. Cost-benefit ratio

### Step 8: Residual Risk Determination

After proposed controls:
- Calculate remaining risk
- Document risk acceptance by appropriate authority
- Define monitoring requirements

---

## Assessment Tiers

### Tier 1: Quick Triage (15-30 min)

**When to use:**
- Low-criticality systems
- Internal tools only
- No sensitive data
- Limited blast radius

**Scope:**
- Lethal Trifecta check (mandatory)
- High-level architecture review
- Top 5 threat scenarios
- Critical control gaps only

**Output:** 1-2 page summary with go/no-go recommendation

**Assumptions:** Documented liberally, flagged for validation if system escalates

### Tier 2: Standard Assessment (1-2 hours)

**When to use:**
- Business-critical systems
- Internal confidential data
- Moderate blast radius

**Scope:**
- Full system characterization
- Complete STRIDE threat model
- OWASP Agentic Top 10 mapping (if applicable)
- Security checklist (authentication, authorization, encryption, logging)
- Remediation roadmap with owners

**Output:** Full risk assessment document

**Assumptions:** Clarifying questions for critical gaps; documented assumptions for minor gaps

### Tier 3: Deep Dive (half-day+)

**When to use:**
- Lethal Trifecta present (all three factors)
- Strictly confidential data
- External-facing systems
- Regulatory scope (HIPAA, PCI, SOX, etc.)

**Scope:**
- Everything in Tier 2, plus:
- Detailed attack path analysis
- Data flow diagrams with trust boundaries
- Control effectiveness testing recommendations
- Compliance mapping

**Output:** Comprehensive assessment + executive briefing summary

---

## Key Definitions

| Term | Definition |
|------|------------|
| **Threat** | Any circumstance or event with potential to adversely impact the system |
| **Threat Source** | The intent and method targeted at exploiting a vulnerability |
| **Vulnerability** | A weakness that could be exploited by a threat source |
| **Risk** | Measure of potential harm, typically likelihood × impact |
| **Control** | Safeguard or countermeasure to reduce risk |
| **Residual Risk** | Risk remaining after controls are applied |
| **Risk Tolerance** | Level of risk the organization is willing to accept |

---

## References

- NIST SP 800-30 Rev 1: Guide for Conducting Risk Assessments
- NIST SP 800-39: Managing Information Security Risk
- NIST IR 8596: Cybersecurity Framework Profile for AI
- OWASP Top 10 for LLM Applications 2025
- OWASP Top 10 for Agentic Applications 2026
