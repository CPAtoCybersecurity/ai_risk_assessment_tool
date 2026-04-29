# AI Risk Assessment Tool — Five-Platform Redesign

**Date:** 2026-04-29
**Status:** Active (supersedes [2026-01-29-ai-risk-assessment-tool-design.md](2026-01-29-ai-risk-assessment-tool-design.md))
**Author:** GRC Team

---

## Executive Summary

The AI Risk Assessment Tool is repackaged from a two-target design (Claude Code skills + Microsoft Copilot Studio) to a **five-platform deployment** model that lets any GRC professional install the agent on whichever AI they already pay for:

1. Microsoft 365 Copilot Agent Builder
2. Claude Project
3. Gemini Gem
4. Custom GPT (ChatGPT)
5. Claude Code Skill (power users)

The methodology, knowledge base, and output format are unchanged. The architecture, terminology, and packaging are all reworked.

---

## Why this redesign

### What changed since January 2026

1. **Wrong Microsoft target.** The January design pointed at **Microsoft Copilot Studio** (copilotstudio.microsoft.com), a Power Platform product requiring separate licensing and Power Platform expertise. The right target for a GRC pro inside an M365-Copilot-enabled org is **Microsoft 365 Copilot Agent Builder** — the in-product, declarative agent experience inside M365 Copilot itself. Different product, different URL, different click path, different knowledge model.

2. **Wrong audience framing.** The January README led with "Option 1: Claude Code (Recommended)" — a developer-grade, command-line-driven path. The OSS audience for this tool is GRC professionals, not Claude Code power users. Leading with Claude Code mis-targets the reader.

3. **Three more consumer platforms exist and matter.** Claude Projects, Gemini Gems, and Custom GPTs are the three other declarative-agent surfaces every GRC pro is likely to have access to. The architecture (system prompt + knowledge files + starter prompts + optional capabilities) is functionally identical across all four consumer platforms — there's no reason to ship to one and not the others.

### The architectural insight

The four consumer platforms share a near-identical agent shape:

```
agent = { instructions: text, knowledge: [files], starter_prompts: [text], capabilities: [...] }
```

So this is one agent definition with four (five with Claude Code) packaging targets — the same problem as a multi-target software build. The right architecture is **single source of truth in `core/`, per-platform packaging in `platforms/<name>/`**. No content forks.

---

## Architecture

### Repo layout

```
ai_risk_assessment_tool/
├── README.md                              ← reader-facing: pick a platform
├── CLAUDE.md                              ← architecture brief for AI editors
├── core/                                   ← single source of truth
│   ├── instructions.md                    ← canonical system prompt (≤7,500 chars)
│   ├── starter-prompts.md                 ← 4 shared conversation starters
│   └── knowledge/                         ← 5 methodology assets, markdown only
│       ├── methodology.md
│       ├── security-checklist.md
│       ├── threat-library.md
│       ├── stride-reference.md
│       └── output-template.md
├── platforms/                              ← packaging per target
│   ├── m365-copilot-agent-builder/README.md
│   ├── claude-project/README.md
│   ├── gemini-gem/README.md
│   ├── custom-gpt/README.md
│   └── claude-code/README.md
├── assessments/                            ← real-world example outputs
└── docs/
    ├── plans/
    │   ├── 2026-01-29-ai-risk-assessment-tool-design.md   ← historical
    │   └── 2026-04-29-five-platform-design.md             ← this doc
    └── platform-comparison.md
```

### Single-source rule

Each platform README references `core/` by relative path; no content is duplicated into platform directories. Edits to methodology, instructions, or knowledge land in exactly one place.

### Character budget

`core/instructions.md` is capped at **7,500 characters**. Tightest platform constraint is Custom GPT and M365 Agent Builder at ~8,000. The 500-character buffer covers paste-fidelity issues and per-org additions. Verified at design time: current canonical instructions = 7,163 chars.

### Format choice

Markdown only. All five platforms accept markdown, four of them ingest it natively, and M365 Agent Builder accepts it as plain text. If M365 ingest quality bites in production, we add a one-time pandoc render (`.md` → `.pdf`) as a build step — not a maintenance burden on the canonical content.

---

## Decisions and rationale

| Decision | Rationale |
|----------|-----------|
| **Drop Microsoft Copilot Studio entirely** | Different product, different audience, redundant given M365 Agent Builder coverage. Re-add as Phase-2 enterprise option only if demand emerges. |
| **Keep Claude Code Skill (5th platform), but de-emphasize** | Power-user audience overlaps with Steve's PAI users. Listed in the platform table but not as the recommended default. |
| **Markdown-only knowledge** | One canonical format. M365 quality is acceptable; no incremental burden of multi-format publishing. |
| **Agent name = "AI Risk Assessment"** (no "Agent" suffix) | Shorter for platform cards; the description carries the agent framing. |
| **No actions / API integrations in MVP** | Each platform has a different action model (M365 connectors vs. OpenAI Actions vs. Claude tools) — premature to design for all four. Phase 2. |
| **Web search ON, code interpreter OFF, image gen OFF** | Web search lets the agent cite current OWASP/NIST. Code interpreter and image gen add attack surface without value for assessment work. |
| **Direct push to main on CPAtoCybersecurity** | Solo development; review burden via PR is overhead for a single-author refactor. |
| **Repo-level CLAUDE.md** | Future AI-assisted edits will not drift back to the old "Copilot Studio" framing or duplicate content into platform directories. |

---

## Methodology — unchanged

The Lethal Trifecta, NIST 800-30 framework, NIST CSF 2.0 control IDs, OWASP LLM/Agentic mappings, STRIDE per-component application, three-tier model, and final assessment template are **carried forward verbatim** from the January 2026 design.

This redesign is packaging, not methodology. No risk of regression on the analytical core.

---

## Migration impact

### Files moved

| From | To |
|------|----|
| `assets/methodology.md` | `core/knowledge/methodology.md` |
| `assets/security-checklist.md` | `core/knowledge/security-checklist.md` |
| `assets/threat-library.md` | `core/knowledge/threat-library.md` |
| `assets/stride-reference.md` | `core/knowledge/stride-reference.md` |
| `assets/output-template.md` | `core/knowledge/output-template.md` |
| `copilot-m365/` | `platforms/m365-copilot-agent-builder/` |

### Files deleted

- `copilot-m365/knowledge-files/*.txt` (five condensed text mirrors — redundant under markdown-canonical model)
- `copilot-m365/system-prompt.md` (replaced by single canonical `core/instructions.md`)
- Old `copilot-m365/README.md` (rewritten as `platforms/m365-copilot-agent-builder/README.md` with corrected terminology)

### Files added

- `core/instructions.md` — canonical system prompt
- `core/starter-prompts.md` — 4 shared starters
- `platforms/{claude-project,gemini-gem,custom-gpt,claude-code}/README.md` — four new deployment guides
- `CLAUDE.md` — architecture brief for AI editors
- `docs/platform-comparison.md` — decision matrix
- `docs/plans/2026-04-29-five-platform-design.md` — this doc

### Files unchanged

- `LICENSE` (MIT)
- `assessments/GRC-News-Assistant-3-risk-assessment-2026-01-29.md`
- `docs/plans/2026-01-29-ai-risk-assessment-tool-design.md` (kept as history)
- All five methodology files (relocated, content unchanged)

---

## Risks and how we're handling them

| Risk | Mitigation |
|------|------------|
| Five platforms × UI churn | Maintenance is per-platform README only; canonical content is one file set. Estimated ~30 min/platform per major UI change. |
| Markdown ingest quality on M365 | Accept slight fidelity loss; add pandoc-render fallback only if a real complaint surfaces. |
| Instructions overrun the 7,500-char budget over time | `wc -c core/instructions.md` is part of every commit-time check. New behavior goes into knowledge files, not instructions. |
| Vendor brand drift (Copilot Studio vs. Agent Builder rename, Gemini Gem rebrand, etc.) | CLAUDE.md captures the canonical names; reviewers will catch drifted references in PRs. |
| Audience licensing reality (every platform requires a paid AI tier) | README is explicit about license requirements; the matrix lets users self-select to whichever they already have. |

---

## Phase-2 roadmap (deferred)

These are deliberately out of scope for the initial five-platform release:

- **Custom GPT Actions** — OpenAPI bridge to ServiceNow GRC, Jira, or SharePoint
- **M365 Power Automate flow** — auto-post finished assessment to a SharePoint list or Teams channel
- **Claude Code orchestration** — multi-skill chaining with code-review and architecture-doc skills
- **Industry overlays** — HIPAA, PCI-DSS, SOC 2, ISO 42001 control packs as separate `core/knowledge/overlays/` files
- **Translations** — methodology-md i18n
- **Optional pandoc render** — auto-generate `.pdf` versions of `core/knowledge/` for M365 ingest if quality complaints arise
- **RAG for the source PDFs** — the agent currently summarizes NIST/OWASP rather than retrieving from full documents; an embedding-based approach could deepen citations

---

## Done means

- All five platform READMEs land complete with deployment steps, troubleshooting, and maintenance instructions
- `core/instructions.md` ≤ 7,500 characters and tested in at least one platform
- README.md leads with the platform-comparison table, not Claude Code
- All "Microsoft Copilot Studio" references removed; "Microsoft 365 Copilot Agent Builder" used consistently
- CLAUDE.md present at repo root capturing the single-source-of-truth principle
- Old design doc retained at `docs/plans/2026-01-29-ai-risk-assessment-tool-design.md` for history
- Branch merged to `main` on `CPAtoCybersecurity/ai_risk_assessment_tool`

---

## References

- January 2026 design: [2026-01-29-ai-risk-assessment-tool-design.md](2026-01-29-ai-risk-assessment-tool-design.md)
- Top-level reader doc: [`README.md`](../../README.md)
- AI-editor brief: [`CLAUDE.md`](../../CLAUDE.md)
- Platform decision matrix: [`docs/platform-comparison.md`](../platform-comparison.md)
