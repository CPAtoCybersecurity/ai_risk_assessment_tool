# AI/Agentic Threat Library

This library provides common threat scenarios for AI and agentic systems, organized by system type and mapped to OWASP Top 10 for LLM Applications (LLM01-10) and OWASP Top 10 for Agentic Applications (ASI01-10).

---

## OWASP Reference Quick Guide

### LLM Top 10 (2025)
| ID | Name |
|----|------|
| LLM01 | Prompt Injection |
| LLM02 | Sensitive Information Disclosure |
| LLM03 | Supply Chain |
| LLM04 | Data and Model Poisoning |
| LLM05 | Improper Output Handling |
| LLM06 | Excessive Agency |
| LLM07 | System Prompt Leakage |
| LLM08 | Vector and Embedding Weaknesses |
| LLM09 | Misinformation |
| LLM10 | Unbounded Consumption |

### Agentic Top 10 (2026)
| ID | Name |
|----|------|
| ASI01 | Agent Goal Hijack |
| ASI02 | Tool Misuse and Exploitation |
| ASI03 | Identity and Privilege Abuse |
| ASI04 | Agentic Supply Chain Vulnerabilities |
| ASI05 | Unexpected Code Execution (RCE) |
| ASI06 | Memory and Context Poisoning |
| ASI07 | Insecure Inter-Agent Communication |
| ASI08 | Cascading Failures |
| ASI09 | Human-Agent Trust Exploitation |
| ASI10 | Rogue Agents |

---

## Threats by System Type

### 1. Chatbots and Conversational AI

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Direct prompt injection | User crafts input to bypass safety guardrails or extract system prompt | E | LLM01, LLM07 | High | Moderate |
| PII disclosure | Bot reveals personal data from context or training | I | LLM02 | Moderate | High |
| Jailbreaking | User manipulates bot to produce harmful/restricted content | E | LLM01 | High | Moderate |
| Session data leakage | Bot reveals information from other users' sessions | I | LLM02 | Low | High |
| Resource exhaustion | Attacker floods chat with requests to exhaust quotas | D | LLM10 | Moderate | Moderate |
| Misinformation generation | Bot confidently provides incorrect information | - | LLM09 | High | Variable |

### 2. RAG (Retrieval-Augmented Generation) Systems

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Indirect prompt injection via documents | Malicious content in indexed documents alters behavior | E | LLM01 | High | High |
| Knowledge base poisoning | Attacker injects false information into vector store | T | LLM04, LLM08 | Moderate | High |
| Access control bypass | RAG retrieves documents user shouldn't access | I, E | LLM08 | Moderate | High |
| Embedding manipulation | Adversarial inputs craft to exploit embedding similarity | T | LLM08 | Low | Moderate |
| Source attribution failure | System cites non-existent or fabricated sources | - | LLM09 | Moderate | Moderate |
| Data exfiltration via retrieval | Sensitive data exposed through retrieval results | I | LLM02 | Moderate | High |

### 3. Agentic Systems (Tool-Using AI)

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Goal hijacking | External input redirects agent's objectives | E | ASI01 | High | Critical |
| Tool misuse | Agent uses legitimate tools for unintended purposes | E | ASI02, LLM06 | High | High |
| Privilege escalation | Agent acquires permissions beyond intended scope | E | ASI03 | Moderate | Critical |
| Infinite loop/runaway | Agent enters unbounded iteration consuming resources | D | LLM10, ASI08 | Moderate | High |
| Unintended code execution | Agent generates and runs malicious code | E | ASI05 | Moderate | Critical |
| Memory poisoning | Attacker corrupts agent's persistent memory | T | ASI06 | Moderate | High |
| Tool output manipulation | External service returns malicious data to agent | T | ASI02 | Moderate | High |

### 4. Multi-Agent Systems

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Agent impersonation | Malicious agent pretends to be trusted agent | S | ASI07 | Moderate | Critical |
| Confused deputy | Low-privilege agent tricks high-privilege agent | E | ASI03 | Moderate | Critical |
| Cascading failures | Failure in one agent propagates to others | D | ASI08 | Moderate | High |
| Message tampering | Inter-agent messages modified in transit | T | ASI07 | Low | High |
| Credential inheritance abuse | Agent inherits excessive permissions from delegator | E | ASI03 | High | High |
| Rogue agent behavior | Agent develops unintended autonomous behavior | E | ASI10 | Low | Critical |
| Cross-agent data leakage | Sensitive data shared inappropriately between agents | I | ASI07, LLM02 | Moderate | High |

### 5. Code Generation / Coding Assistants

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Insecure code generation | AI produces code with vulnerabilities | T | LLM05 | High | High |
| Backdoor injection | Malicious code patterns embedded in suggestions | T | LLM04, ASI04 | Low | Critical |
| Credential exposure | Generated code includes hardcoded secrets | I | LLM02 | Moderate | High |
| Dependency confusion | AI suggests malicious packages (typosquatting) | T | LLM03, ASI04 | Moderate | High |
| Command injection | Generated shell commands executed unsafely | E | ASI05 | Moderate | Critical |
| Workspace compromise | Coding agent gains excessive filesystem access | E | ASI05 | Moderate | High |

### 6. Email/Document Processing Agents

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Email-borne prompt injection | Malicious email contains hidden instructions | E | LLM01, ASI01 | High | Critical |
| Attachment payload execution | Processing malicious attachments triggers actions | E | ASI05 | Moderate | Critical |
| Exfiltration via reply | Agent sends sensitive data in automated replies | I | LLM02 | Moderate | High |
| Calendar injection | Malicious calendar invites alter agent behavior | E | ASI01 | Moderate | High |
| Phishing amplification | Agent forwards or acts on phishing content | S | ASI09 | Moderate | High |
| Document-based data theft | Processing documents triggers data exfiltration | I | LLM01 | High | High |

### 7. Customer Service / Support Agents

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Social engineering via AI | Attacker manipulates agent to reveal customer data | I | LLM02 | Moderate | High |
| Unauthorized account actions | Agent performs actions without proper verification | E | LLM06 | Moderate | High |
| Refund/discount abuse | Agent tricked into issuing unauthorized refunds | E | ASI02 | Moderate | High |
| Brand impersonation | Responses don't align with brand guidelines | - | LLM09 | Moderate | Moderate |
| Competitor information leakage | Agent reveals internal business information | I | LLM02 | Low | High |
| Escalation bypass | Agent handles issues it should escalate to humans | E | LLM06, ASI09 | Moderate | Moderate |

### 8. Financial / Trading Systems

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| Fraudulent transaction execution | Agent manipulated to transfer funds | E | ASI01, ASI02 | Moderate | Critical |
| Market manipulation | Agent acts on false market signals | T | LLM04 | Low | Critical |
| Audit trail manipulation | Agent actions not properly logged | R | - | Low | High |
| Unauthorized trading | Agent executes trades beyond authorized limits | E | LLM06 | Low | Critical |
| Insider information leakage | Agent reveals non-public financial data | I | LLM02 | Low | Critical |

### 9. Healthcare / Medical AI

| Threat | Description | STRIDE | OWASP | Likelihood | Impact |
|--------|-------------|--------|-------|------------|--------|
| PHI disclosure | Protected health information exposed | I | LLM02 | Moderate | Critical |
| Incorrect medical advice | AI provides harmful medical recommendations | - | LLM09 | Moderate | Critical |
| Treatment plan manipulation | Attacker influences AI treatment suggestions | T | LLM04, ASI01 | Low | Critical |
| HIPAA compliance violation | System handling doesn't meet regulatory requirements | I | LLM02 | Moderate | High |
| Medical record tampering | AI-assisted modifications to patient records | T | ASI02 | Low | Critical |

---

## Threat Scenarios by Attack Vector

### External Input Vectors

| Vector | Associated Threats | OWASP |
|--------|-------------------|-------|
| Web forms / chat | Direct prompt injection, jailbreaking | LLM01 |
| Uploaded documents | Indirect prompt injection, payload execution | LLM01, ASI05 |
| Email content | Email-borne injection, phishing | ASI01 |
| API calls | Input manipulation, rate abuse | LLM01, LLM10 |
| Webhooks | Event-driven injection, replay attacks | ASI01 |
| Calendar invites | Goal manipulation, scheduling attacks | ASI01 |
| External websites | Content-based injection in browsing agents | LLM01, ASI01 |

### Supply Chain Vectors

| Vector | Associated Threats | OWASP |
|--------|-------------------|-------|
| Model weights | Backdoored models, poisoned training | LLM03, LLM04 |
| RAG knowledge bases | Knowledge poisoning, false information | LLM08, LLM04 |
| MCP servers/tools | Malicious tool definitions, typosquatting | ASI04 |
| Prompt templates | Injected instructions in templates | ASI04 |
| Agent registries | Fake agent cards, impersonation | ASI04, ASI07 |
| NPM/PyPI packages | Dependency attacks on coding agents | ASI04 |

---

## Lethal Trifecta Threat Combinations

When all three factors of the Lethal Trifecta are present, these combined threats become especially dangerous:

| Scenario | Private Data | Untrusted Input | External Comm | Result |
|----------|-------------|-----------------|---------------|--------|
| Email agent with database access | Customer DB | Incoming emails | Outbound email | Attacker emails hidden instruction → agent queries DB → exfiltrates via reply |
| Slack bot with file access | Internal files | Slack messages | Slack posting | Malicious message → bot reads confidential files → posts to public channel |
| RAG chatbot with API access | Business data | Web form | Third-party API | Poisoned document → chatbot extracts data → sends to attacker's endpoint |
| Code agent with repo access | Source code | PR comments | GitHub API | Injection in PR → agent reads secrets → commits to attacker's repo |

---

## References

- OWASP Top 10 for LLM Applications 2025
- OWASP Top 10 for Agentic Applications 2026
- NIST SP 800-30 Appendix E: Threat Events
- MITRE ATLAS (Adversarial Threat Landscape for AI Systems)
