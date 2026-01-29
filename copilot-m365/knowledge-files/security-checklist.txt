# AI Security Checklist (Condensed)

## Critical Controls

### Authentication & Access
- [ ] User authentication required
- [ ] MFA for privileged access
- [ ] RBAC implemented
- [ ] Least privilege enforced
- [ ] API authentication required
- [ ] Agent identities distinct from users

### Data Protection
- [ ] Data classification applied
- [ ] Encryption at rest
- [ ] Encryption in transit (TLS 1.2+)
- [ ] Secrets in vault (not code)
- [ ] PII/PHI policy compliant

### Input Validation
- [ ] External input validated
- [ ] SQL injection prevention
- [ ] Command injection prevention
- [ ] Rate limiting enabled

### AI/LLM Controls

**Prompt Injection Defense:**
- [ ] System prompts protected
- [ ] User input isolated from instructions
- [ ] Untrusted content delimited
- [ ] Output validated before action

**Tool Controls:**
- [ ] Tool permissions minimized
- [ ] Destructive actions require confirmation
- [ ] Tool outputs validated
- [ ] Tools from trusted sources only

**Data Exposure Prevention:**
- [ ] Context excludes unauthorized data
- [ ] RAG respects access controls
- [ ] Sensitive data redacted

**Agent Autonomy:**
- [ ] Human-in-the-loop for high-impact
- [ ] Loop/iteration limits set
- [ ] Resource/spending caps
- [ ] Agent cannot self-modify permissions

### Output Controls
- [ ] Output sanitized (XSS prevention)
- [ ] Sensitive data redacted
- [ ] External comms gated
- [ ] Generated code sandboxed

### Logging & Monitoring
- [ ] Auth events logged
- [ ] All agent actions logged
- [ ] Tool invocations logged
- [ ] Anomaly alerting configured

### Incident Response
- [ ] Kill switch exists
- [ ] IR runbook documented
- [ ] Rollback procedure ready

## Quick Assessment

| Area | Pass | Fail | N/A |
|------|:----:|:----:|:---:|
| Authentication | | | |
| Data Protection | | | |
| Input Validation | | | |
| AI/LLM Controls | | | |
| Output Controls | | | |
| Logging | | | |
| Incident Response | | | |
