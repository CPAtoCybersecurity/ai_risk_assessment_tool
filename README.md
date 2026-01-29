# AI Risk Assessment Tool

An open-source AI agent for conducting structured risk assessments of AI and agentic systems, following NIST SP 800-30 methodology with the NIST Cybersecurity Framework Profile for AI (Cyber AI Profile) and AI-specific controls from OWASP.

## Overview

This tool helps GRC professionals assess AI systems before production deployment by:

- **Lethal Trifecta Analysis** - Early-gate check for high-risk combinations (private data + untrusted input + external communication)
- **Tiered Assessment** - Right-sized evaluation based on system criticality
- **STRIDE Threat Modeling** - Systematic threat identification per component
- **OWASP Mapping** - Cross-reference to LLM Top 10 and Agentic Top 10
- **Structured Output** - Consistent risk assessment documents

## Deployment Options

### Option 1: Claude Code (Recommended)

Composable skills for flexible, deep assessments.

**Installation:**

```bash
# Copy skills to Claude Code skills directory
cp -r assets/ ~/.claude/skills/risk-assessment/
```

**Usage:**

```
/risk-assessment    # Full guided assessment
/risk-triage        # Quick Lethal Trifecta check
/threat-model       # STRIDE analysis only
/risk-report        # Generate document from existing analysis
```

### Option 2: Microsoft Copilot Studio

Monolithic agent for enterprise M365 environments.

**Deployment:**

1. Open [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Create new agent
3. Paste instructions from `copilot-m365/system-prompt.md`
4. Upload files from `copilot-m365/knowledge-files/`
5. Publish to Teams or web

See [copilot-m365/README.md](copilot-m365/README.md) for detailed instructions.

## Directory Structure

```
ai-risk-assessment-tool/
├── README.md                 # This file
├── assets/                   # Core methodology files
│   ├── methodology.md        # Risk framework, tiers, scales
│   ├── security-checklist.md # 85 controls with NIST CSF mapping
│   ├── output-template.md    # Assessment document template
│   ├── threat-library.md     # Threats by system type
│   └── stride-reference.md   # STRIDE with AI examples
├── assessments/              # Example outputs
│   └── GRC-News-Assistant-3-risk-assessment-2026-01-29.md
├── copilot-m365/             # Microsoft Copilot deployment
│   ├── README.md
│   ├── system-prompt.md
│   └── knowledge-files/      # Condensed .txt versions for Copilot
└── docs/plans/               # Design documentation
    └── 2026-01-29-ai-risk-assessment-tool-design.md
```

## Key Concepts

### The Lethal Trifecta

Systems with ALL THREE factors require Tier 3 (most rigorous) assessment:

| Factor | Risk |
|--------|------|
| **Private Data Access** | Can AI access databases, emails, files, credentials? |
| **Untrusted Input** | Can external parties send data via webhooks, emails, APIs? |
| **External Communication** | Can AI send HTTP requests, emails, post externally? |

### Assessment Tiers

| Tier | Duration | Use When |
|------|----------|----------|
| **1** | 15-30 min | Low criticality, internal only, no sensitive data |
| **2** | 1-2 hours | Business-critical, confidential data |
| **3** | Half-day+ | Lethal Trifecta present, regulated data |

### Risk Framework

Based on NIST SP 800-30 and the NIST Cybersecurity Framework Profile for AI (IR 8596), with extensions for agentic systems:

- **Likelihood**: Very Low → Very High (5 levels)
- **Impact**: Very Low → Very High (5 levels)
- **Risk**: Low / Medium / High / Critical

## Reference Documents

This tool synthesizes guidance from the following authoritative sources. Users should download these for reference:

| Document | Source |
|----------|--------|
| NIST SP 800-30 Rev 1 | [NIST CSRC](https://csrc.nist.gov/publications/detail/sp/800-30/rev-1/final) |
| NIST CSF Profile for AI (Cyber AI Profile) | [NIST IR 8596](https://csrc.nist.gov/pubs/ir/8596/iprd) |
| NIST AI RMF Generative AI Profile | [NIST AI](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence) |
| OWASP Top 10 for LLM Applications 2025 | [OWASP](https://owasp.org/www-project-top-10-for-large-language-model-applications/) |
| OWASP Top 10 for Agentic Applications 2026 | [OWASP](https://owasp.org/www-project-top-10-for-large-language-model-applications/) |
| MCP Server Security Cheat Sheet | [Arcanium Security](https://github.com/Arcanum-Sec/sec-context) |

## Example Assessment

See [assessments/GRC-News-Assistant-3-risk-assessment-2026-01-29.md](assessments/GRC-News-Assistant-3-risk-assessment-2026-01-29.md) for a complete assessment of [GRC News Assistant 3](https://github.com/CPAtoCybersecurity/GRC_News_Assistant_3), an n8n-based news aggregation workflow with AI processing.

**Highlights from the example:**
- Lethal Trifecta: 2 of 3 factors (no private data access)
- Overall Risk: LOW-MEDIUM
- Recommendation: Approve with monitoring
- 7 threat scenarios identified and rated

## Contributing

Contributions welcome! Areas for improvement:

- Additional threat scenarios for emerging AI architectures
- Industry-specific control mappings (HIPAA, PCI-DSS, SOC 2)
- Integration with GRC platforms
- Automated evidence collection

## License

MIT License - see [LICENSE](LICENSE) for details.

## Acknowledgments

- [NIST](https://www.nist.gov/) for SP 800-30, AI RMF, and Cyber AI Profile (IR 8596)
- [OWASP](https://owasp.org/) for LLM and Agentic security guidance
- [Daniel Miessler](https://github.com/danielmiessler/fabric) for Fabric patterns inspiration
- [Arcanium Security](https://github.com/Arcanum-Sec/sec-context) for MCP security context
