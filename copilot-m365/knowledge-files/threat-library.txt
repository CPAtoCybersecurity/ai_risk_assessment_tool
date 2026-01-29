# AI Threat Library

## OWASP Top 10 for LLM (2025)

| ID | Threat | Description |
|----|--------|-------------|
| LLM01 | Prompt Injection | Malicious input alters behavior |
| LLM02 | Sensitive Info Disclosure | Data leaks via outputs |
| LLM03 | Supply Chain | Compromised dependencies |
| LLM04 | Data/Model Poisoning | Corrupted training data |
| LLM05 | Improper Output Handling | Unsafe output processing |
| LLM06 | Excessive Agency | Too much autonomy |
| LLM07 | System Prompt Leakage | Prompt exposure |
| LLM08 | Vector/Embedding Weaknesses | RAG vulnerabilities |
| LLM09 | Misinformation | False outputs |
| LLM10 | Unbounded Consumption | Resource exhaustion |

## OWASP Top 10 for Agentic (2026)

| ID | Threat | Description |
|----|--------|-------------|
| ASI01 | Agent Goal Hijack | Redirected objectives |
| ASI02 | Tool Misuse | Legitimate tools abused |
| ASI03 | Identity/Privilege Abuse | Permission escalation |
| ASI04 | Agentic Supply Chain | Compromised tools/agents |
| ASI05 | Unexpected Code Execution | RCE via generation |
| ASI06 | Memory/Context Poisoning | Corrupted agent state |
| ASI07 | Insecure Inter-Agent Comm | Agent-to-agent attacks |
| ASI08 | Cascading Failures | Multi-agent collapse |
| ASI09 | Human-Agent Trust Exploitation | Social engineering via AI |
| ASI10 | Rogue Agents | Autonomous misalignment |

## Common Threats by System Type

### Chatbots
- Direct prompt injection (LLM01)
- PII disclosure (LLM02)
- Jailbreaking (LLM01)

### RAG Systems
- Indirect injection via documents (LLM01)
- Knowledge poisoning (LLM04, LLM08)
- Access control bypass (LLM08)

### Agentic Systems
- Goal hijacking (ASI01)
- Tool misuse (ASI02, LLM06)
- Privilege escalation (ASI03)
- Runaway loops (LLM10, ASI08)

### Multi-Agent
- Agent impersonation (ASI07)
- Confused deputy (ASI03)
- Cascading failures (ASI08)

### Code Generation
- Insecure code output (LLM05)
- Dependency confusion (LLM03, ASI04)
- Command injection (ASI05)

### Email/Doc Processing
- Email-borne injection (LLM01, ASI01)
- Exfiltration via reply (LLM02)
- Attachment payload execution (ASI05)

## Lethal Trifecta Combinations

When all three factors present:

| Scenario | Attack Path |
|----------|-------------|
| Email agent + DB | Injected email → query DB → exfiltrate via reply |
| Slack bot + files | Malicious message → read files → post to channel |
| RAG + APIs | Poisoned doc → extract data → send to attacker |
