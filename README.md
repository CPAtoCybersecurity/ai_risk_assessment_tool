# AI Risk Assessment

An open-source AI agent for conducting structured risk assessments of AI and agentic systems, following NIST SP 800-30 methodology with the NIST Cybersecurity Framework Profile for AI (NIST IR 8596) and AI-specific controls from OWASP.

**One agent. Five platforms. Pick the AI you already pay for.**

## What it does

This tool helps GRC professionals assess AI systems before production deployment by:

- **Lethal Trifecta Analysis** — Mandatory early gate for high-risk combinations (private data + untrusted input + external communication)
- **Tiered Assessment** — Right-sized evaluation based on system criticality (Tier 1 / 2 / 3)
- **STRIDE Threat Modeling** — Systematic threat identification per architectural component
- **OWASP Mapping** — Cross-reference to LLM Top 10 (2025) and Agentic Top 10 (2026)
- **NIST CSF 2.0 Controls** — 85 controls mapped to CSF function IDs (GV / ID / PR / DE / RS / RC)
- **Structured Output** — Consistent risk assessment documents with approval workflow

## Pick your platform

| Platform | License needed | Best for | Install time |
|----------|----------------|----------|--------------|
| **[Microsoft 365 Copilot Agent Builder](platforms/m365-copilot-agent-builder/README.md)** | M365 Copilot | Enterprise users already in M365 | 5 min |
| **[Claude Project](platforms/claude-project/README.md)** | Claude Pro / Team / Max | Long-context assessments, markdown-native | 3 min |
| **[Gemini Gem](platforms/gemini-gem/README.md)** | Google AI Pro / Workspace | Google-shop GRC teams | 5 min |
| **[Custom GPT](platforms/custom-gpt/README.md)** | ChatGPT Plus / Team / Enterprise | Most familiar to GRC pros, cleanest sharing | 5 min |
| **[Claude Code Skill](platforms/claude-code/README.md)** | Claude Pro / Max / Team | Power users on the CLI / VS Code | 2 min |

Same methodology, same knowledge base, same output format on every platform.

## How it's organized

```
ai_risk_assessment_tool/
├── README.md                              ← you are here
├── CLAUDE.md                              ← architecture for AI assistants editing this repo
├── core/                                   ← single source of truth (DRY)
│   ├── instructions.md                    ← canonical system prompt — paste into every platform
│   ├── starter-prompts.md                 ← 4 shared conversation starters
│   └── knowledge/                         ← 5 methodology files — upload to every platform
│       ├── methodology.md                  ←     risk framework, tiers, scales
│       ├── security-checklist.md           ←     85 controls with NIST CSF 2.0 IDs
│       ├── threat-library.md               ←     threats by system type
│       ├── stride-reference.md             ←     STRIDE with AI examples
│       └── output-template.md              ←     final document template
├── platforms/                              ← deployment guide per platform
│   ├── m365-copilot-agent-builder/
│   ├── claude-project/
│   ├── gemini-gem/
│   ├── custom-gpt/
│   └── claude-code/
├── assessments/                            ← real-world example outputs
└── docs/                                   ← design docs, platform comparison
```

**Update once, deploy anywhere.** Edit `core/` — every platform's deployment guide tells you how to refresh that platform with the new content.

## Key concepts

### The Lethal Trifecta

Systems with **all three** factors require Tier 3 (most rigorous) assessment:

- **Private data access** — Can the AI access databases, emails, files, credentials?
- **Untrusted input** — Can external parties send data via webhooks, emails, public APIs?
- **External communication** — Can the AI send HTTP requests, post externally?

If all three are present *and* the architecture has no isolating controls, the agent recommends **redesign before further assessment.**

### Assessment tiers

| Tier | Duration | Use when |
|------|----------|----------|
| **1** | 15–30 min | Low criticality, internal only, no sensitive data |
| **2** | 1–2 hours | Business-critical, confidential data |
| **3** | Half-day+ | Lethal Trifecta present, regulated data, external-facing |

### Risk framework

Based on NIST SP 800-30 Rev 1 and NIST IR 8596 (Cyber AI Profile), with extensions for agentic systems:

- **Likelihood:** Very Low → Very High (5 levels)
- **Impact:** Very Low → Very High (5 levels)
- **Risk:** Likelihood × Impact → Low / Medium / High / Critical

## Reference documents

This tool synthesizes guidance from these authoritative sources:

| Document | Source |
|----------|--------|
| NIST SP 800-30 Rev 1 | <https://csrc.nist.gov/publications/detail/sp/800-30/rev-1/final> |
| NIST CSF Profile for AI (Cyber AI Profile) | <https://csrc.nist.gov/pubs/ir/8596/iprd> |
| NIST AI RMF Generative AI Profile | <https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence> |
| OWASP Top 10 for LLM Applications 2025 | <https://owasp.org/www-project-top-10-for-large-language-model-applications/> |
| OWASP Top 10 for Agentic Applications 2026 | <https://owasp.org/www-project-top-10-for-large-language-model-applications/> |
| MCP Server Security Cheat Sheet | <https://github.com/Arcanum-Sec/sec-context> |

## Example assessment

See [`assessments/GRC-News-Assistant-3-risk-assessment-2026-01-29.md`](assessments/GRC-News-Assistant-3-risk-assessment-2026-01-29.md) for a complete assessment of [GRC News Assistant 3](https://github.com/CPAtoCybersecurity/GRC_News_Assistant_3), an n8n-based news aggregation workflow with AI processing.

**Highlights:**
- Lethal Trifecta: 2 of 3 factors (no private data access)
- Overall Risk: LOW-MEDIUM
- Recommendation: Approve with monitoring
- 7 threat scenarios identified and rated

## When to use which platform

See [`docs/platform-comparison.md`](docs/platform-comparison.md) for the decision matrix.

Quick rule of thumb:

- **You're at a Microsoft shop and your users live in Teams** → M365 Copilot Agent Builder
- **You want the deepest, longest-context conversations** → Claude Project
- **You're at a Google shop / Workspace user** → Gemini Gem
- **You want the broadest reach and the cleanest sharing model** → Custom GPT
- **You're a developer and live in the terminal** → Claude Code skill

## Contributing

Contributions welcome:

- Additional threat scenarios for emerging AI architectures
- Industry-specific control mappings (HIPAA, PCI-DSS, SOC 2, ISO 42001)
- Phase-2 platform integrations (ServiceNow, Jira, SharePoint)
- Translations of the methodology

Please don't contribute platform-specific forks of the system prompt — keep `core/instructions.md` as the single source of truth.

## License

MIT — see [LICENSE](LICENSE).

## Acknowledgments

- [NIST](https://www.nist.gov/) for SP 800-30, AI RMF, and Cyber AI Profile (IR 8596)
- [OWASP](https://owasp.org/) for LLM and Agentic security guidance
- [Daniel Miessler](https://github.com/danielmiessler/fabric) for Fabric patterns inspiration
- [Arcanium Security](https://github.com/Arcanum-Sec/sec-context) for MCP security context
