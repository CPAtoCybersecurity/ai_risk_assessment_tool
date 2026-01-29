# STRIDE Quick Reference

## Categories

| Letter | Threat | Violates | Key Question |
|:------:|--------|----------|--------------|
| **S** | Spoofing | Authentication | Can identity be faked? |
| **T** | Tampering | Integrity | Can data be modified? |
| **R** | Repudiation | Non-repudiation | Can actions be denied? |
| **I** | Info Disclosure | Confidentiality | Can data leak? |
| **D** | Denial of Service | Availability | Can service be disrupted? |
| **E** | Elevation of Privilege | Authorization | Can access be escalated? |

## AI-Specific Examples

### Spoofing (S)
- Agent impersonates another agent
- Forged agent identities
- Synthetic identity injection

### Tampering (T)
- Poisoned training data
- Modified RAG knowledge
- Tampered tool definitions
- Corrupted agent memory

### Repudiation (R)
- Agent actions without attribution
- Missing tool invocation logs
- Unclear delegation chains

### Info Disclosure (I)
- System prompt leakage
- PII in outputs
- Cross-session data leaks
- Exfiltration via tool calls

### Denial of Service (D)
- Infinite agent loops
- Token/cost exhaustion
- Cascading multi-agent failures

### Elevation of Privilege (E)
- Prompt injection bypass
- Tool permission escalation
- Jailbreaking guardrails
- Confused deputy attacks

## Analysis Template

For each component, ask:

| STRIDE | Threat? | Control? |
|:------:|---------|----------|
| S | | |
| T | | |
| R | | |
| I | | |
| D | | |
| E | | |
