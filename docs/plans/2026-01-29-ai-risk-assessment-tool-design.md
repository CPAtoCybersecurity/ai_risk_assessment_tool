# AI Risk Assessment Tool - Design Document

**Date:** 2026-01-29
**Status:** Draft
**Author:** GRC Team

---

## Executive Summary

An AI-powered risk assessment tool for internal GRC analysts to evaluate AI/agentic systems before production approval. The tool produces structured risk assessment documents following NIST 800-30 methodology, incorporating STRIDE threat modeling and AI-specific security controls.

**Two deployment targets:**
- **Claude Code (Mac)** – Composable skills with RAG capability
- **Copilot M365 (Windows)** – Monolithic agent with attached knowledge files

Both share a common methodology, output format, and security checklist.

---

## Target User

**Internal GRC analyst** assessing systems before production approval.

**Workflow characteristics:**
- Mixed intake methods (forms, documents, interviews)
- Full governance lifecycle downstream (approvals, risk acceptance, ticketing)
- Tiered assessment depth based on system criticality

---

## 1. Shared Methodology Core

### Risk Assessment Framework (NIST 800-30)

1. **System Characterization** – Boundaries, components, data flows, users, integrations.

2. **Asset Identification** – Data classification (Public → Strictly Confidential), critical functions, dependencies.

3. **Lethal Trifecta Analysis** – Mandatory early gate for AI/agentic systems:

   | Factor | Present? | Details |
   |--------|----------|---------|
   | **Private data access** | ☐ Yes ☐ No | Databases, emails, APIs, files, credentials |
   | **Untrusted input** | ☐ Yes ☐ No | Webhooks, emails, Slack, public APIs, user content |
   | **External communication** | ☐ Yes ☐ No | HTTP requests, posting to GitHub/Slack, sending emails |

   **If all three present → Elevated risk tier. Requires explicit architectural controls or redesign.**

4. **Threat Identification** – STRIDE per component + OWASP Top 10 for Agentic Applications.

5. **Vulnerability Analysis** – Maps to sec-context anti-patterns, control gaps.

6. **Likelihood × Impact → Risk Rating** (Low/Medium/High/Critical)

7. **Control Recommendations** – Prioritized by risk reduction and feasibility.

8. **Residual Risk** – Formally accepted by risk owner.

---

## 2. Assessment Tiers

### Tier 1: Quick Triage (15-30 min)

**When to use:** Low-criticality systems, internal tools, no sensitive data, limited blast radius

**Scope:**
- Lethal Trifecta check (mandatory)
- High-level architecture review
- Top 5 threat scenarios
- Critical control gaps only

**Output:** 1-2 page summary with go/no-go recommendation

**Assumptions:** Documented liberally, flagged for validation if system escalates

### Tier 2: Standard Assessment (1-2 hours)

**When to use:** Business-critical systems, internal confidential data, moderate blast radius

**Scope:**
- Full system characterization
- Complete STRIDE threat model
- OWASP Agentic Top 10 mapping (if applicable)
- Security checklist (authentication, authorization, encryption, logging)
- Remediation roadmap with owners

**Output:** Full risk assessment document

**Assumptions:** Clarifying questions for critical gaps; documented assumptions for minor gaps

### Tier 3: Deep Dive (half-day+)

**When to use:** Systems with Lethal Trifecta, strictly confidential data, external-facing, regulatory scope

**Scope:**
- Everything in Tier 2, plus:
- Detailed attack path analysis
- Data flow diagrams with trust boundaries
- Control effectiveness testing recommendations
- Compliance mapping (if applicable)

**Output:** Comprehensive assessment + executive briefing summary

---

## 3. Output Document Template

```markdown
# [System Name] Risk Assessment

**Assessment Date:** YYYY-MM-DD
**Assessor:** [Name]
**Tier:** 1 / 2 / 3
**Status:** Draft / Pending Approval / Approved / Risk Accepted

## Executive Summary

[2-3 sentences: What was assessed, key findings, overall risk rating, recommendation]

## Lethal Trifecta Check

| Factor | Present | Details |
|--------|---------|---------|
| Private data access | ☐ Yes ☐ No | |
| Untrusted input | ☐ Yes ☐ No | |
| External communication | ☐ Yes ☐ No | |

**Trifecta Status:** ⚠️ All three present / ✓ Partial or none

## Business Objective

[Why does this system exist? What problem does it solve?]

## Use Cases

[Primary user workflows and interactions]

## Architecture & Deployment

### Architecture / Data Flow Diagram

[Mermaid diagram or image reference]

### Authentication

[How users/systems authenticate]

### Trust Boundaries

[Where trusted meets untrusted]

### Assets

| Asset | Classification | Description |
|-------|----------------|-------------|
| | Public / Internal / Confidential / Strictly Confidential | |

## Threat Scenarios

**Overall Residual Risk:** [Low / Medium / High / Critical]

| ID | Threat Scenario | Likelihood | Impact | Risk | STRIDE |
|----|-----------------|------------|--------|------|--------|
| T1 | | | | | |

### Analysis Notes

[Context on threat modeling decisions and assumptions]

## Remediation Actions

| ID | Action | Owner | Due Date | Status | Linked Ticket |
|----|--------|-------|----------|--------|---------------|
| R1 | | | | Not Started / In Progress / Complete | |

## Security Checklist

[See Section 4 for full checklist]

## Approval

| Role | Name | Decision | Date | Signature |
|------|------|----------|------|-----------|
| Assessor | | Complete | | |
| Security Lead | | Approve / Reject / Risk Accept | | |
| Risk Owner | | Accept Residual Risk | | |

## Appendix

### Assumptions Made

[List assumptions and items requiring validation]

### References

[NIST 800-30 sections, OWASP mappings, etc.]
```

---

## 4. Security Checklist with NIST CSF 2.0 Mapping

### Authentication & Access Control

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| PR.AA-03 | User authentication required | ☐ | ☐ | ☐ | |
| PR.AA-03 | Multi-factor authentication for privileged access | ☐ | ☐ | ☐ | |
| PR.AA-05 | Role-based access control (RBAC) implemented | ☐ | ☐ | ☐ | |
| PR.AA-05 | Principle of least privilege enforced | ☐ | ☐ | ☐ | |
| PR.AA-01 | Service accounts use scoped credentials | ☐ | ☐ | ☐ | |
| PR.AA-05 | Session timeout configured | ☐ | ☐ | ☐ | |
| PR.AA-03 | API authentication required (keys, OAuth, etc.) | ☐ | ☐ | ☐ | |

### Data Protection

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| ID.AM-07 | Data classification applied to all assets | ☐ | ☐ | ☐ | |
| PR.DS-01 | Encryption at rest for sensitive data | ☐ | ☐ | ☐ | |
| PR.DS-02 | Encryption in transit (TLS 1.2+) | ☐ | ☐ | ☐ | |
| PR.DS-01 | Credentials/secrets stored in vault (not code) | ☐ | ☐ | ☐ | |
| PR.DS-01 | PII/PHI handling compliant with policy | ☐ | ☐ | ☐ | |
| GV.PO-01 | Data retention policy defined | ☐ | ☐ | ☐ | |
| PR.IR-04 | Backup and recovery procedures documented | ☐ | ☐ | ☐ | |

### Input Validation & Injection Prevention

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| PR.PS-06 | All external input validated and sanitized | ☐ | ☐ | ☐ | |
| PR.PS-06 | SQL/NoSQL injection prevention (parameterized queries) | ☐ | ☐ | ☐ | |
| PR.PS-06 | Command injection prevention | ☐ | ☐ | ☐ | |
| PR.PS-06 | Path traversal prevention | ☐ | ☐ | ☐ | |
| PR.PS-06 | File upload restrictions (type, size, scanning) | ☐ | ☐ | ☐ | |
| PR.IR-04 | Rate limiting on input endpoints | ☐ | ☐ | ☐ | |

### AI/LLM-Specific Controls

#### Prompt Injection Defense

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| PR.PS-06 | System prompts protected from user manipulation | ☐ | ☐ | ☐ | |
| PR.DS-10 | User input isolated from system instructions | ☐ | ☐ | ☐ | |
| PR.PS-06 | Untrusted content marked/delimited clearly | ☐ | ☐ | ☐ | |

#### Tool/Function Calling Controls

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| PR.AA-05 | Tool permissions scoped to minimum necessary | ☐ | ☐ | ☐ | |
| PR.AA-06 | Destructive actions require confirmation | ☐ | ☐ | ☐ | |
| PR.DS-10 | Tool outputs validated before use | ☐ | ☐ | ☐ | |

#### Data Exposure Prevention

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| PR.AA-05 | Context window excludes unauthorized data | ☐ | ☐ | ☐ | |
| PR.AA-05 | RAG retrieval respects access controls | ☐ | ☐ | ☐ | |
| PR.AA-05 | Model cannot access data beyond user's permissions | ☐ | ☐ | ☐ | |

#### Agent Autonomy Limits

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| GV.RM-07 | Human-in-the-loop for high-impact actions | ☐ | ☐ | ☐ | |
| PR.IR-04 | Maximum iteration/loop limits configured | ☐ | ☐ | ☐ | |
| GV.RM-07 | Spending/resource caps in place | ☐ | ☐ | ☐ | |
| PR.AA-05 | Agent cannot self-modify permissions | ☐ | ☐ | ☐ | |

### Output Controls

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| PR.PS-06 | Output sanitized before rendering (XSS prevention) | ☐ | ☐ | ☐ | |
| PR.DS-01 | Sensitive data redacted from responses | ☐ | ☐ | ☐ | |
| PR.DS-02 | External communications reviewed/gated | ☐ | ☐ | ☐ | |
| PR.PS-05 | Generated code/commands sandboxed before execution | ☐ | ☐ | ☐ | |
| PR.IR-04 | Response size limits configured | ☐ | ☐ | ☐ | |

### Third-Party & Supply Chain

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| GV.SC-04 | Third-party dependencies inventoried | ☐ | ☐ | ☐ | |
| GV.SC-04 | Dependency vulnerability scanning enabled | ☐ | ☐ | ☐ | |
| GV.SC-06 | MCP servers/plugins from trusted sources | ☐ | ☐ | ☐ | |
| GV.SC-05 | Third-party API permissions reviewed | ☐ | ☐ | ☐ | |
| GV.SC-05 | Model provider data handling policy reviewed | ☐ | ☐ | ☐ | |

### Logging & Monitoring

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| DE.CM-01 | Authentication events logged | ☐ | ☐ | ☐ | |
| DE.CM-01 | All AI/agent actions logged with context | ☐ | ☐ | ☐ | |
| DE.CM-01 | Tool invocations logged | ☐ | ☐ | ☐ | |
| PR.DS-01 | Sensitive data excluded from logs | ☐ | ☐ | ☐ | |
| PR.PS-04 | Log retention meets compliance requirements | ☐ | ☐ | ☐ | |
| DE.AE-06 | Alerting configured for anomalous behavior | ☐ | ☐ | ☐ | |
| PR.DS-01 | Logs protected from tampering | ☐ | ☐ | ☐ | |

### Infrastructure & Deployment

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| PR.IR-01 | Network segmentation appropriate | ☐ | ☐ | ☐ | |
| PR.IR-01 | Firewall/security groups configured | ☐ | ☐ | ☐ | |
| PR.PS-01 | Container/runtime hardened | ☐ | ☐ | ☐ | |
| PR.DS-01 | Secrets not in environment variables or code | ☐ | ☐ | ☐ | |
| PR.PS-06 | CI/CD pipeline includes security scanning | ☐ | ☐ | ☐ | |
| PR.AA-05 | Production access restricted and audited | ☐ | ☐ | ☐ | |

### Incident Response

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|-----|-----|-----|----------|
| RS.MI-01 | Kill switch/disable capability exists | ☐ | ☐ | ☐ | |
| RS.MA-01 | Incident response runbook documented | ☐ | ☐ | ☐ | |
| RS.CO-02 | Data breach notification process defined | ☐ | ☐ | ☐ | |
| RC.RP-01 | Rollback procedure documented | ☐ | ☐ | ☐ | |

### CSF 2.0 Functions Referenced

- **GV** (Govern): GV.PO, GV.RM, GV.SC
- **ID** (Identify): ID.AM
- **PR** (Protect): PR.AA, PR.DS, PR.IR, PR.PS
- **DE** (Detect): DE.AE, DE.CM
- **RS** (Respond): RS.CO, RS.MA, RS.MI
- **RC** (Recover): RC.RP

---

## 5. Claude Code Architecture (Mac)

### Skill Structure

```
~/.claude/skills/
└── risk-assessment/
    ├── risk-assessment.md        # Meta-skill (orchestrates full workflow)
    ├── risk-intake.md            # Gather system information
    ├── risk-triage.md            # Lethal Trifecta + tier determination
    ├── threat-model.md           # STRIDE analysis
    ├── risk-analyze.md           # Likelihood/impact/risk ratings
    ├── risk-report.md            # Generate final document
    └── assets/
        ├── methodology.md        # Shared framework
        ├── output-template.md    # Document template
        ├── security-checklist.md # Full checklist with CSF IDs
        ├── threat-library.md     # Common threat scenarios
        └── stride-reference.md   # STRIDE quick reference
```

### Skill Flow

```
┌─────────────────────────────────────────────────────────┐
│  /risk-assessment (meta-skill)                          │
│  Orchestrates full workflow, or user invokes individual │
└─────────────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────┐     ┌─────────────────┐
│ /risk-intake    │────▶│ /risk-triage    │
│ Gather info     │     │ Lethal Trifecta │
│ from docs/chat  │     │ + select tier   │
└─────────────────┘     └─────────────────┘
                                 │
         ┌───────────────────────┴───────────────────────┐
         ▼                                               ▼
┌─────────────────┐                             ┌─────────────────┐
│ /threat-model   │                             │ Security        │
│ STRIDE analysis │                             │ Checklist       │
│ per component   │                             │ walkthrough     │
└─────────────────┘                             └─────────────────┘
         │                                               │
         └───────────────────────┬───────────────────────┘
                                 ▼
                        ┌─────────────────┐
                        │ /risk-analyze   │
                        │ Rate risks,     │
                        │ residual risk   │
                        └─────────────────┘
                                 │
                                 ▼
                        ┌─────────────────┐
                        │ /risk-report    │
                        │ Generate final  │
                        │ document        │
                        └─────────────────┘
```

### Knowledge Integration

Skills read markdown assets directly using Claude's native file reading. No vector database required for MVP.

---

## 6. Copilot M365 Architecture (Windows)

### Agent Configuration

```
Name: AI Risk Assessment Agent
Description: Conducts structured risk assessments for AI/agentic
             systems following NIST 800-30 methodology

Knowledge Files (attached):
├── methodology.md           # Framework, tiers, Lethal Trifecta
├── output-template.md       # Document structure
├── security-checklist.md    # Full checklist with CSF IDs
├── threat-library.md        # Common threat scenarios
├── nist-800-30-summary.md   # Key excerpts
├── owasp-agentic-top10.md   # Summarized
└── stride-reference.md      # STRIDE quick reference
```

### System Prompt

```markdown
# AI Risk Assessment Agent

You are an internal GRC analyst assistant that conducts structured
risk assessments for AI and agentic systems.

## Your Process

1. **Intake**: Gather system information through conversation. Ask about:
   - System name and business purpose
   - Architecture (components, integrations, data flows)
   - Data handled (classification levels)
   - Users and access model
   - Deployment environment

2. **Lethal Trifecta Check** (MANDATORY FIRST):
   Immediately assess:
   - Private data access? (databases, emails, APIs, files)
   - Untrusted input? (webhooks, emails, Slack, public APIs)
   - External communication? (HTTP requests, posting externally)

   If ALL THREE present → Flag elevated risk, recommend Tier 3.

3. **Tier Selection**: Based on trifecta + system criticality:
   - Tier 1: Quick triage (low criticality, no sensitive data)
   - Tier 2: Standard (business-critical, internal confidential)
   - Tier 3: Deep dive (trifecta present, strictly confidential, external-facing)

4. **Threat Modeling**: Apply STRIDE to each component.
   Reference threat-library.md for common scenarios.

5. **Security Checklist**: Walk through controls appropriate to tier.
   Reference security-checklist.md.

6. **Risk Rating**: Likelihood × Impact → Risk level.

7. **Output**: Generate assessment document using output-template.md format.

## Interaction Style

- Ask ONE question at a time
- Prefer multiple choice when possible
- Make documented assumptions for minor gaps
- Ask clarifying questions for critical gaps
- Be direct and professional

## References

Consult attached knowledge files for methodology, threats, controls, and output format.
```

### Platform Comparison

| Aspect | Claude Code | Copilot M365 |
|--------|-------------|--------------|
| Orchestration | User invokes skills | Agent manages flow |
| Knowledge | Direct file reading | Attached files |
| Depth | Can go deeper on demand | Bounded by context |
| Flexibility | Mix/match skills | Fixed workflow |
| Output | Writes to local files | Conversation + copy/paste |

---

## 7. Knowledge Base Preparation

### Source Materials

| File | Use For |
|------|---------|
| nistspecialpublication800-30r1.pdf | Risk methodology, likelihood/impact scales |
| OWASP-Top-10-for-Agentic-Applications-2026-12.6-1.pdf | AI-specific threats |
| NIST.IR.8596.iprd.pdf | AI system considerations |
| LLMAll_en-US_FINAL.pdf | LLM security patterns |
| ChearSheet-A-Practical-Guide-for-Securely-Using-third-party-MCP-Servers1.0.pdf | MCP-specific controls |

### Shared Asset Files

1. **methodology.md** (~2,000 words) – Framework, Lethal Trifecta, tiers, scales
2. **security-checklist.md** (~1,500 words) – Full checklist with CSF IDs
3. **output-template.md** (~500 words) – Document structure
4. **threat-library.md** (~2,500 words) – Scenarios by system type
5. **stride-reference.md** (~500 words) – STRIDE definitions and examples

---

## 8. Implementation Roadmap

### Phase 1: Foundation (Core Assets)

- [ ] Create `methodology.md`
- [ ] Create `security-checklist.md`
- [ ] Create `output-template.md`
- [ ] Create `threat-library.md`
- [ ] Create `stride-reference.md`
- [ ] Extract key content from PDFs into markdown

### Phase 2: Claude Code Skills (Mac)

- [ ] Build `/risk-intake` skill
- [ ] Build `/risk-triage` skill
- [ ] Build `/threat-model` skill
- [ ] Build `/risk-analyze` skill
- [ ] Build `/risk-report` skill
- [ ] Build `/risk-assessment` meta-skill
- [ ] Test end-to-end

### Phase 3: Copilot M365 Agent (Windows)

- [ ] Condense assets for context limits
- [ ] Write consolidated system prompt
- [ ] Configure agent in Copilot Studio
- [ ] Attach knowledge files
- [ ] Test and iterate

### Phase 4: Refinement

- [ ] Test against real assessments
- [ ] Tune threat library
- [ ] Add organization-specific controls
- [ ] Document usage guide
- [ ] Gather feedback

### Future Enhancements

- RAG integration for deep PDF queries
- Ticketing system integration (Jira/ServiceNow)
- Diagram generation (Mermaid)
- Risk register integration
- Approval workflow automation

---

## Appendix: References

- NIST SP 800-30 Rev 1: Guide for Conducting Risk Assessments
- NIST CSF 2.0: Cybersecurity Framework
- OWASP Top 10 for Agentic Applications 2026
- Fabric Patterns: create_stride_threat_model, create_threat_scenarios
- sec-context: AI Code Security Anti-Patterns Guide
