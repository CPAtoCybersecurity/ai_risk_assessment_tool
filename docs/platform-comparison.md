# Platform Comparison — Choosing Your Deployment Target

All five platforms run the same agent (same `core/instructions.md`, same `core/knowledge/`, same output format). The differences are about **which AI you already pay for**, where your team works, and a handful of platform-specific quirks.

## Decision matrix

| Question | Pick this platform |
|----------|-------------------|
| "My org runs on Microsoft 365 and our users live in Teams." | **M365 Copilot Agent Builder** |
| "I want the deepest analysis on long architecture documents." | **Claude Project** |
| "We're a Google Workspace shop." | **Gemini Gem** |
| "I want the broadest reach and the easiest sharing." | **Custom GPT** |
| "I'm a developer; the agent should live next to my code." | **Claude Code Skill** |
| "I'm not sure — what's the safest default?" | **Custom GPT** (most users have ChatGPT Plus, cleanest sharing) |

## Side-by-side comparison

| Aspect | M365 Copilot Agent Builder | Claude Project | Gemini Gem | Custom GPT | Claude Code Skill |
|--------|---------------------------|----------------|------------|------------|-------------------|
| **License** | M365 Copilot | Claude Pro/Team/Max | Google AI Pro / Workspace | ChatGPT Plus/Team/Enterprise | Claude Pro/Max/Team |
| **Approx cost** | $30/user/mo (M365 Copilot add-on) | $20–$200/user/mo | $20+/user/mo | $20–$60/user/mo | $20–$200/user/mo |
| **Install time** | ~5 min | ~3 min | ~5 min | ~5 min | ~2 min |
| **Markdown handling** | OK (text) | Native | OK (variable) | Native | Native |
| **Long-context strength** | Moderate | **Strongest** | Strong | Strong | **Strongest** |
| **Knowledge upload** | Files + SharePoint + Graph | Files + connectors | Files (rolling out) | Files (very robust) | Direct filesystem |
| **Sharing** | Tenant / Teams / users | Project share (Team plan) | Workspace internal | Link / GPT Store | Local only |
| **Actions / API integrations** | Limited (Power Platform) | Connectors | Limited | **OpenAI Actions (OpenAPI)** | Anything (full toolchain) |
| **Web search** | Yes | Yes (connector) | Yes | Yes | Yes (with browser tooling) |
| **Best for non-developer GRC pro** | ✅ if M365 shop | ✅ | ✅ if Google shop | ✅ best default | ❌ |
| **Best for power user / dev** | OK | ✅ | OK | ✅ | ✅ |

## Trade-offs worth knowing

### Microsoft 365 Copilot Agent Builder

**Strengths:** Native to M365 — your users already authenticate, knowledge sources can pull from SharePoint and Graph, agents publish to Teams and Outlook in one click.

**Weaknesses:** Markdown ingest is OK but not as crisp as Claude/GPT; the in-product Agent Builder has more limited action support than full Copilot Studio (which we deliberately don't target). Tenant-internal by default — you can't share publicly.

**Watch for:** Don't confuse Agent Builder with Copilot Studio. Studio is a separate Power Platform product; we don't deploy there.

### Claude Project

**Strengths:** Markdown is native; long-context is Claude's signature strength — entire methodologies, full architecture docs, and complete checklists fit in working memory without retrieval gaps. Custom Instructions field is generous.

**Weaknesses:** No native conversation-starter UI (use the project description instead). Sharing requires a Team plan.

**Watch for:** When a single conversation gets very long, start a new chat in the same project — knowledge persists, conversation state resets.

### Gemini Gem

**Strengths:** Excellent long-context model. Workspace-internal sharing for Google shops. Free tier of Gemini may be adequate for personal use even before paid Gems.

**Weaknesses:** Knowledge file upload has been rolling out unevenly across accounts — the inline-knowledge fallback (append knowledge to the Instructions field) is a reliable workaround but adds maintenance friction.

**Watch for:** Gemini tends to be more verbose by default. The instructions explicitly include "be direct and professional," but you may need to add an extra "be concise" line if responses run long.

### Custom GPT

**Strengths:** Most users already have ChatGPT Plus — lowest friction to access. Knowledge files handle markdown well, up to 20 files / 2M tokens each. Conversation Starters render cleanly. **OpenAI Actions** is the cleanest path to Phase-2 integrations (ServiceNow, Jira, SharePoint).

**Weaknesses:** Instructions cap at 8,000 characters (we sit at ~7,200, so headroom is tight if you add org-specific extensions).

**Watch for:** "Anyone with the link" is the right sharing default for internal GRC use — public GPT Store listing requires verified-builder profile and exposes the prompt.

### Claude Code Skill

**Strengths:** No upload, no retrieval, no character limit — Claude reads the canonical knowledge files directly from disk. Composable with the rest of your Claude Code skills (e.g., chain with documentation-writing or code-review skills). Lives in git, version-controlled.

**Weaknesses:** Power-user only. Not shareable with non-developer GRC analysts. Requires Claude Code installed and configured.

**Watch for:** The simplest install (copy `core/` into `~/.claude/skills/risk-assessment/`) creates a single mega-skill. If you want per-phase commands (`/risk-triage`, `/threat-model`), create separate skill directories — see the platform's README.

## Hybrid deployments

Nothing prevents running the agent on multiple platforms simultaneously:

- **GRC analysts on Custom GPT** for routine intake assessments
- **Security architecture team on Claude Project** for deep-dive Tier 3 reviews
- **Power users on Claude Code Skill** for code-base-level analysis

All three would draw from the same `core/` content. Updates to methodology propagate via the maintenance step in each platform's README.

## What we deliberately don't target

- **Microsoft Copilot Studio** (the standalone Power Platform product) — different audience, heavier setup, redundant given M365 Copilot Agent Builder coverage.
- **Open-source self-hosted** (Ollama, vLLM, LM Studio) — current methodology assumes frontier-model reasoning quality; smaller local models tend to skip the trifecta gate or hallucinate control IDs.
- **API-only deployments** (raw Anthropic/OpenAI/Google API) — these would bypass the per-platform UX (knowledge upload, starter prompts) that makes the agent usable by non-developer GRC pros. If you need API access, the canonical `core/instructions.md` is the system prompt; build whatever harness you like around it.

## Future platform candidates

If a new platform meets all of these, it's worth adding to the matrix:

- Declarative agent configuration (instructions + knowledge, no code required)
- Reasonable price point (≤$50/user/mo for most users in the target audience)
- Knowledge-file upload supporting at least markdown or PDF
- Generally available, not invite-only beta
- Tenant or org-scoped sharing
