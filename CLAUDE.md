# AI Risk Assessment Tool — Architecture for AI Editors

> This file is for AI assistants (Claude, Cursor, Copilot, etc.) working in this repo. It explains the repo's architecture so future edits don't drift back to older designs.

## What this repo is

An open-source AI agent that conducts structured risk assessments for AI and agentic systems. The same agent can be deployed on five different AI platforms — that is the central design constraint.

## The five-platform model

The agent ships to:

1. **Microsoft 365 Copilot Agent Builder** — the in-product, declarative agent experience inside M365 Copilot. **NOT** Microsoft Copilot Studio (a different Power Platform product). If you see a reference to "Copilot Studio" in this repo, it's a bug — fix it to "Microsoft 365 Copilot Agent Builder."
2. **Claude Project** — claude.ai project with custom instructions + project knowledge.
3. **Gemini Gem** — custom Gem at gemini.google.com/gems.
4. **Custom GPT** — chatgpt.com Custom GPT.
5. **Claude Code Skill** — power-user terminal/IDE deployment via the PAI skills system.

## Single-source-of-truth principle

Content lives in `core/`. Platforms package; they never fork content.

```
core/
├── instructions.md         ← canonical system prompt (≤7,500 chars, paste into every platform)
├── starter-prompts.md      ← 4 shared conversation starters
└── knowledge/              ← 5 markdown files uploaded to every platform's knowledge layer
    ├── methodology.md
    ├── security-checklist.md
    ├── threat-library.md
    ├── stride-reference.md
    └── output-template.md
```

**When you edit content:**

- Change to *what the agent does, says, or knows* → edit a file in `core/`.
- Change to *how the agent is installed on a specific platform* → edit `platforms/<platform>/README.md`.
- Never duplicate content from `core/` into a platform directory. Every platform README references `core/instructions.md` and `core/knowledge/*.md` by relative path.

## Character budget

`core/instructions.md` must stay **≤ 7,500 characters** so it fits within the tightest platform's instructions limit (Custom GPT and M365 Agent Builder both cap at ~8,000 — 7,500 leaves headroom for paste fidelity and per-org additions). Always check with `wc -c core/instructions.md` after edits.

If a behavioral addition would push the file over 7,500, move the supporting detail to a new knowledge file in `core/knowledge/` instead and reference it from the instructions.

## Methodology grounding (do not change without justification)

The agent's methodology is grounded in:

- **NIST SP 800-30 Rev 1** — risk-assessment framework, likelihood × impact scales
- **NIST CSF 2.0** — every control in `security-checklist.md` carries a CSF function ID (GV / ID / PR / DE / RS / RC)
- **NIST IR 8596** — Cyber AI Profile (AI-specific control overlays)
- **OWASP Top 10 for LLM Applications (2025)** — referenced in threat scenarios
- **OWASP Top 10 for Agentic Applications (2026)** — referenced in threat scenarios
- **STRIDE** — applied per architectural component
- **The Lethal Trifecta** — the mandatory early-gate concept (private data + untrusted input + external communication)

These five anchors define the agent's conceptual frame. Don't replace them with alternative methodologies (e.g., FAIR, OCTAVE) without an explicit decision recorded in `docs/plans/`.

## Adding a new platform

If a new AI platform emerges and you want to add it:

1. Create `platforms/<new-platform>/README.md`.
2. Follow the structure of the existing five (basic settings → instructions → knowledge → starter prompts → capabilities → test → maintenance → troubleshooting).
3. Reference `core/instructions.md` and `core/knowledge/*.md` — do not copy them into the new platform directory.
4. Add a row to the comparison table in the top-level `README.md`.
5. Add a row to `docs/platform-comparison.md`.

## Adding a control or threat

To add a security control or threat scenario:

- **New control** → edit `core/knowledge/security-checklist.md`. Always include a NIST CSF 2.0 ID.
- **New threat scenario** → edit `core/knowledge/threat-library.md`. Always include the STRIDE category and an OWASP mapping.
- **New methodology section** → edit `core/knowledge/methodology.md`.
- **Output structure change** → edit `core/knowledge/output-template.md`.

After editing, verify the agent still runs correctly on at least one platform (Claude Project is fastest to test).

## What NOT to do

- ❌ Don't add a sixth path that bypasses `core/` (no inline-prompt-only platforms).
- ❌ Don't shorten or paraphrase `core/instructions.md` differently for each platform — keep it identical.
- ❌ Don't introduce vendor-specific syntax (e.g., Microsoft adaptive cards, OpenAI function-calling JSON) into `core/`. That belongs in `platforms/<platform>/`.
- ❌ Don't replace the Lethal Trifecta concept with anything else — it is the agent's signature analytical frame and the entry point of every assessment.
- ❌ Don't reintroduce "Microsoft Copilot Studio" as a deployment target. It was deliberately dropped in the 2026-04-29 redesign.

## Repo conventions

- Every change that adds a deployment target or alters methodology should land a corresponding entry in `docs/plans/`.
- Markdown only (no HTML, no LaTeX, no proprietary formats) — the content must paste cleanly into five different platform UIs.
- Tables use GitHub-flavored markdown. Avoid wide tables (>5 columns) — they wrap badly in some platforms' rendered output.
- Code blocks are fenced with backticks; specify language for syntax highlighting where helpful.

## Reference

- Top-level orientation: `README.md`
- Authoritative design doc: `docs/plans/2026-04-29-five-platform-design.md`
- Platform comparison: `docs/platform-comparison.md`
- Historical design (kept for context): `docs/plans/2026-01-29-ai-risk-assessment-tool-design.md`
