# AI/Agentic Security Checklist

This checklist covers security controls for AI and agentic systems, mapped to NIST CSF 2.0 subcategories.

**Usage:** For each control, mark Yes (implemented), No (not implemented), or N/A (not applicable). Add comments for context.

---

## 1. Authentication & Access Control

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.AA-03 | User authentication required | | | | |
| PR.AA-03 | Multi-factor authentication for privileged access | | | | |
| PR.AA-05 | Role-based access control (RBAC) implemented | | | | |
| PR.AA-05 | Principle of least privilege enforced | | | | |
| PR.AA-01 | Service accounts use scoped credentials | | | | |
| PR.AA-05 | Session timeout configured | | | | |
| PR.AA-03 | API authentication required (keys, OAuth, etc.) | | | | |
| PR.AA-05 | Agent identities are distinct from user identities | | | | |
| PR.AA-05 | Permissions are per-task/per-session, not permanent | | | | |

---

## 2. Data Protection

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| ID.AM-07 | Data classification applied to all assets | | | | |
| PR.DS-01 | Encryption at rest for sensitive data | | | | |
| PR.DS-02 | Encryption in transit (TLS 1.2+) | | | | |
| PR.DS-01 | Credentials/secrets stored in vault (not code) | | | | |
| PR.DS-01 | PII/PHI handling compliant with policy | | | | |
| GV.PO-01 | Data retention policy defined | | | | |
| PR.IR-04 | Backup and recovery procedures documented | | | | |

---

## 3. Input Validation & Injection Prevention

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.PS-06 | All external input validated and sanitized | | | | |
| PR.PS-06 | SQL/NoSQL injection prevention (parameterized queries) | | | | |
| PR.PS-06 | Command injection prevention | | | | |
| PR.PS-06 | Path traversal prevention | | | | |
| PR.PS-06 | File upload restrictions (type, size, scanning) | | | | |
| PR.IR-04 | Rate limiting on input endpoints | | | | |

---

## 4. AI/LLM-Specific Controls

### 4.1 Prompt Injection Defense

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.PS-06 | System prompts protected from user manipulation | | | | |
| PR.DS-10 | User input isolated from system instructions | | | | |
| PR.PS-06 | Untrusted content marked/delimited clearly | | | | |
| PR.PS-06 | Input filtering for known injection patterns | | | | |
| PR.PS-06 | Output validation before acting on LLM responses | | | | |

### 4.2 Tool/Function Calling Controls

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.AA-05 | Tool permissions scoped to minimum necessary | | | | |
| PR.AA-06 | Destructive actions require confirmation | | | | |
| PR.DS-10 | Tool outputs validated before use | | | | |
| PR.AA-05 | Tool invocations rate-limited | | | | |
| PR.AA-05 | Tools cannot modify their own permissions | | | | |
| GV.SC-06 | Tools/MCP servers sourced from trusted sources | | | | |

### 4.3 Data Exposure Prevention

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.AA-05 | Context window excludes unauthorized data | | | | |
| PR.AA-05 | RAG retrieval respects access controls | | | | |
| PR.AA-05 | Model cannot access data beyond user's permissions | | | | |
| PR.DS-01 | Training data does not include production secrets | | | | |
| PR.DS-01 | Sensitive data redacted from model inputs | | | | |

### 4.4 Agent Autonomy Limits

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| GV.RM-07 | Human-in-the-loop for high-impact actions | | | | |
| PR.IR-04 | Maximum iteration/loop limits configured | | | | |
| GV.RM-07 | Spending/resource caps in place | | | | |
| PR.AA-05 | Agent cannot self-modify permissions | | | | |
| GV.RM-07 | Agent scope clearly defined and enforced | | | | |
| PR.IR-04 | Timeout limits for agent execution | | | | |

### 4.5 Multi-Agent Security

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.AA-03 | Inter-agent authentication required | | | | |
| PR.DS-02 | Agent-to-agent communication encrypted | | | | |
| PR.AA-05 | Delegation chains respect least privilege | | | | |
| PR.AA-05 | Agent identities verified before trust | | | | |
| DE.CM-01 | Inter-agent messages logged | | | | |

---

## 5. Output Controls

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.PS-06 | Output sanitized before rendering (XSS prevention) | | | | |
| PR.DS-01 | Sensitive data redacted from responses | | | | |
| PR.DS-02 | External communications reviewed/gated | | | | |
| PR.PS-05 | Generated code/commands sandboxed before execution | | | | |
| PR.IR-04 | Response size limits configured | | | | |
| PR.DS-02 | Egress filtering (allowlist external destinations) | | | | |

---

## 6. Third-Party & Supply Chain

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| GV.SC-04 | Third-party dependencies inventoried | | | | |
| GV.SC-04 | Dependency vulnerability scanning enabled | | | | |
| GV.SC-06 | MCP servers/plugins from trusted sources | | | | |
| GV.SC-05 | Third-party API permissions reviewed | | | | |
| GV.SC-05 | Model provider data handling policy reviewed | | | | |
| GV.SC-04 | SBOM/AIBOM maintained for AI components | | | | |
| GV.SC-06 | Tool definitions pinned to specific versions | | | | |

---

## 7. Logging & Monitoring

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| DE.CM-01 | Authentication events logged | | | | |
| DE.CM-01 | All AI/agent actions logged with context | | | | |
| DE.CM-01 | Tool invocations logged | | | | |
| PR.DS-01 | Sensitive data excluded from logs | | | | |
| PR.PS-04 | Log retention meets compliance requirements | | | | |
| DE.AE-06 | Alerting configured for anomalous behavior | | | | |
| PR.DS-01 | Logs protected from tampering | | | | |
| DE.CM-01 | Prompt/response pairs logged (where appropriate) | | | | |
| DE.CM-01 | Resource consumption tracked | | | | |

---

## 8. Infrastructure & Deployment

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| PR.IR-01 | Network segmentation appropriate | | | | |
| PR.IR-01 | Firewall/security groups configured | | | | |
| PR.PS-01 | Container/runtime hardened | | | | |
| PR.DS-01 | Secrets not in environment variables or code | | | | |
| PR.PS-06 | CI/CD pipeline includes security scanning | | | | |
| PR.AA-05 | Production access restricted and audited | | | | |
| PR.PS-05 | Agent execution sandboxed | | | | |

---

## 9. Incident Response

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| RS.MI-01 | Kill switch/disable capability exists | | | | |
| RS.MA-01 | Incident response runbook documented | | | | |
| RS.CO-02 | Data breach notification process defined | | | | |
| RC.RP-01 | Rollback procedure documented | | | | |
| RS.MI-01 | Ability to revoke agent permissions immediately | | | | |
| RS.MA-01 | AI-specific incident scenarios documented | | | | |

---

## 10. Governance & Compliance

| CSF ID | Control | Yes | No | N/A | Comments |
|--------|---------|:---:|:---:|:---:|----------|
| GV.PO-01 | AI usage policy defined | | | | |
| GV.RM-07 | Risk acceptance criteria documented | | | | |
| GV.OC-01 | Regulatory requirements identified | | | | |
| GV.PO-01 | Terms of service for AI providers reviewed | | | | |
| GV.RM-07 | AI bias and fairness considerations addressed | | | | |
| GV.SC-05 | Data residency requirements met | | | | |

---

## CSF 2.0 Functions Referenced

| Function | Categories Used |
|----------|-----------------|
| **GV** (Govern) | GV.PO, GV.RM, GV.SC, GV.OC |
| **ID** (Identify) | ID.AM |
| **PR** (Protect) | PR.AA, PR.DS, PR.IR, PR.PS |
| **DE** (Detect) | DE.AE, DE.CM |
| **RS** (Respond) | RS.CO, RS.MA, RS.MI |
| **RC** (Recover) | RC.RP |

---

## Checklist Summary

| Section | Total Controls | Yes | No | N/A |
|---------|---------------|-----|-----|-----|
| 1. Authentication & Access | 9 | | | |
| 2. Data Protection | 7 | | | |
| 3. Input Validation | 6 | | | |
| 4. AI/LLM-Specific | 22 | | | |
| 5. Output Controls | 6 | | | |
| 6. Third-Party & Supply Chain | 7 | | | |
| 7. Logging & Monitoring | 9 | | | |
| 8. Infrastructure & Deployment | 7 | | | |
| 9. Incident Response | 6 | | | |
| 10. Governance & Compliance | 6 | | | |
| **TOTAL** | **85** | | | |

---

## References

- NIST Cybersecurity Framework 2.0
- NIST IR 8596: Cybersecurity Framework Profile for AI
- OWASP Top 10 for LLM Applications 2025
- OWASP Top 10 for Agentic Applications 2026
