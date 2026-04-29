# Deploy as a Claude Code Skill

> **Platform:** Claude Code (terminal CLI, VS Code extension, or desktop app) with the PAI skills system.
>
> **License required:** Claude Pro / Max / Team (any plan that runs Claude Code).
>
> **Time to install:** ~2 minutes if you already use Claude Code.
>
> **Audience:** Power users who want a composable, file-driven workflow rather than a single agent. The other four platforms (M365 Copilot, Claude Project, Gemini Gem, Custom GPT) are simpler if you don't already use Claude Code.

## What this looks like in Claude Code

Instead of a single monolithic agent, the methodology becomes **invokable skills**:

| Command | What it does |
|---------|--------------|
| `/risk-assessment` | Full guided assessment — orchestrates the whole flow |
| `/risk-triage` | Quick Lethal Trifecta check + tier recommendation |
| `/threat-model` | STRIDE analysis only |
| `/risk-report` | Generate the final document from existing analysis |

Each skill is a separate markdown file in `~/.claude/skills/risk-assessment/`. Claude Code reads the canonical knowledge files directly from disk — no upload, no retrieval layer, no context-window bottleneck.

## Deployment Steps

### 1. Confirm Claude Code is installed

```bash
claude --version
```

If not installed, see <https://docs.claude.com/claude-code>.

### 2. Install the skill

From the repo root:

```bash
mkdir -p ~/.claude/skills/risk-assessment
cp core/instructions.md ~/.claude/skills/risk-assessment/SKILL.md
cp -r core/knowledge ~/.claude/skills/risk-assessment/knowledge
```

The `SKILL.md` file is what Claude Code loads when the skill is invoked. Knowledge files sit alongside it and are referenced by relative path.

### 3. (Optional) Add separate sub-skills

If you want the per-phase commands (`/risk-triage`, `/threat-model`, `/risk-report`), create them as separate skill files in `~/.claude/skills/`:

```bash
# Example: a focused triage skill
cat > ~/.claude/skills/risk-triage/SKILL.md <<'EOF'
---
name: risk-triage
description: Quick Lethal Trifecta + tier recommendation. Use when only the early gate is needed, not the full assessment.
---

[paste the LETHAL TRIFECTA + TIER SELECTION sections of core/instructions.md]
EOF
```

This is optional — `/risk-assessment` alone covers the full flow.

### 4. Test

```bash
claude
```

Then in Claude Code:

```
/risk-assessment

Assess a new internal chatbot that reads our HR knowledge base.
```

Claude Code should run intake → trifecta → tier → threats → checklist → rating → output, reading `~/.claude/skills/risk-assessment/knowledge/*.md` as needed.

## Updating

When `core/` content updates in this repo:

```bash
cd /path/to/ai_risk_assessment_tool
cp core/instructions.md ~/.claude/skills/risk-assessment/SKILL.md
cp -r core/knowledge/* ~/.claude/skills/risk-assessment/knowledge/
```

Or symlink instead of copy if you prefer live updates:

```bash
rm -rf ~/.claude/skills/risk-assessment
ln -s "$(pwd)/core" ~/.claude/skills/risk-assessment
# (then create a SKILL.md symlink pointing to instructions.md)
```

## When to use this vs. a Claude Project

| Use Claude Code skill if… | Use Claude Project if… |
|---------------------------|------------------------|
| You already work in Claude Code daily | You work mostly in claude.ai chat |
| You want skills composable with other PAI tools | You want a clean, shareable artifact |
| You want to run assessments against local files (architecture docs, code) | You're sharing the agent with non-developer GRC pros |
| You want the skill checked into a git workflow | You want web-based access from any device |

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `/risk-assessment` not recognized | Confirm `~/.claude/skills/risk-assessment/SKILL.md` exists and has YAML frontmatter. |
| Skill loads but ignores knowledge files | Check the relative path inside `SKILL.md` — should reference `knowledge/methodology.md` etc. |
| Want to use across multiple repos | Symlink `~/.claude/skills/risk-assessment` to a central checkout of this repo's `core/`. |
