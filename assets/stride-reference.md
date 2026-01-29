# STRIDE Threat Model Reference

## Overview

STRIDE is a threat modeling framework developed by Microsoft to categorize security threats. Each letter represents a category of threat that can affect a system component.

---

## STRIDE Categories

### S - Spoofing

**Definition:** Pretending to be something or someone other than yourself.

**Violates:** Authentication

**Questions to ask:**
- Can an attacker impersonate a user?
- Can an attacker impersonate the system to users?
- Can an attacker forge authentication tokens or credentials?

**AI/Agentic Examples:**
- Agent impersonates another agent in multi-agent systems
- Forged agent cards or MCP tool descriptors
- Synthetic identity injection in delegation chains
- Prompt injection causing AI to claim false identity

**Mitigations:**
- Strong authentication (MFA, OAuth, mTLS)
- Mutual authentication between agents
- Signed agent identities and attestation
- Cryptographic verification of tool/agent sources

---

### T - Tampering

**Definition:** Modifying data or code without authorization.

**Violates:** Integrity

**Questions to ask:**
- Can data be modified in transit?
- Can data be modified at rest?
- Can configuration or code be altered?

**AI/Agentic Examples:**
- Poisoned training data or fine-tuning datasets
- Modified RAG knowledge bases
- Tampered tool definitions or MCP schemas
- Manipulated agent memory or context
- Compromised prompt templates

**Mitigations:**
- Encryption in transit (TLS 1.2+)
- Integrity verification (checksums, signatures)
- Version control with audit trails
- Input validation and sanitization
- Immutable logs

---

### R - Repudiation

**Definition:** Claiming to have not performed an action.

**Violates:** Non-repudiation

**Questions to ask:**
- Can a user deny performing an action?
- Can the system prove who did what and when?
- Are audit trails complete and tamper-proof?

**AI/Agentic Examples:**
- Agent actions without attribution to requesting user
- Unclear delegation chains in multi-agent workflows
- Missing logs for tool invocations
- AI decisions without explainability trail

**Mitigations:**
- Comprehensive audit logging
- Tamper-evident log storage
- Digital signatures on actions
- User session binding for agent actions
- Intent tracking through delegation chains

---

### I - Information Disclosure

**Definition:** Exposing information to unauthorized parties.

**Violates:** Confidentiality

**Questions to ask:**
- Can sensitive data leak through responses?
- Can system internals be exposed?
- Can data flow to unauthorized destinations?

**AI/Agentic Examples:**
- System prompt leakage
- PII in model outputs
- Training data extraction attacks
- RAG retrieval bypassing access controls
- Cross-session data leakage via agent memory
- Exfiltration through tool calls

**Mitigations:**
- Output filtering and sanitization
- Data classification enforcement
- Access control on RAG retrieval
- Memory isolation between sessions
- Egress controls and allowlists
- Sensitive data redaction

---

### D - Denial of Service

**Definition:** Denying or degrading service to legitimate users.

**Violates:** Availability

**Questions to ask:**
- Can the system be overwhelmed?
- Can resources be exhausted?
- Can the system be crashed or hung?

**AI/Agentic Examples:**
- Infinite agent loops consuming resources
- Token/API cost exhaustion attacks
- Model endpoint flooding
- Context window stuffing
- Cascading failures in multi-agent systems
- Tool rate limit exhaustion

**Mitigations:**
- Rate limiting and throttling
- Resource quotas and budgets
- Loop detection and iteration limits
- Circuit breakers for cascading failures
- Graceful degradation
- Kill switches for runaway processes

---

### E - Elevation of Privilege

**Definition:** Gaining capabilities beyond what was authorized.

**Violates:** Authorization

**Questions to ask:**
- Can a user gain admin rights?
- Can an attacker escalate from low to high privilege?
- Can system boundaries be crossed?

**AI/Agentic Examples:**
- Prompt injection to bypass restrictions
- Agent acquiring inherited permissions via delegation
- Tool permission escalation through chaining
- Jailbreaking model safety guardrails
- Cross-agent trust exploitation (confused deputy)
- Self-modification of agent permissions

**Mitigations:**
- Principle of least privilege
- Per-task scoped permissions
- Human-in-the-loop for privileged actions
- Re-authentication on privilege escalation
- Sandboxed execution environments
- Permission boundary enforcement

---

## STRIDE Per Component Analysis

When applying STRIDE, analyze each component of your system:

| Component | S | T | R | I | D | E |
|-----------|---|---|---|---|---|---|
| User Interface | X | X | X | X | X | X |
| API Gateway | X | X | X | X | X | X |
| LLM/Model | - | X | - | X | X | X |
| Agent Orchestrator | X | X | X | X | X | X |
| Tools/Functions | X | X | X | X | X | X |
| RAG/Vector DB | - | X | - | X | X | - |
| External APIs | X | X | - | X | X | - |
| Data Storage | - | X | - | X | X | - |

---

## Quick Reference Table

| Threat | Property Violated | Typical Controls |
|--------|-------------------|------------------|
| **Spoofing** | Authentication | MFA, certificates, token validation |
| **Tampering** | Integrity | Encryption, signatures, validation |
| **Repudiation** | Non-repudiation | Logging, audit trails, signatures |
| **Info Disclosure** | Confidentiality | Encryption, access control, filtering |
| **Denial of Service** | Availability | Rate limits, redundancy, monitoring |
| **Elevation of Privilege** | Authorization | Least privilege, RBAC, sandboxing |

---

## References

- Microsoft STRIDE Threat Model
- NIST SP 800-30 Threat Sources and Events
- OWASP Threat Modeling
