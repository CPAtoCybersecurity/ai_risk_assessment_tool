# Deploy as a Claude Project

> **Platform:** Claude.ai Project (custom instructions + project knowledge).
>
> **License required:** Claude Pro, Team, or Max plan (Projects feature).
>
> **Time to install:** ~3 minutes. Claude handles markdown natively, so this is the lowest-friction deployment.

## Deployment Steps

### 1. Create the project

1. Sign in to <https://claude.ai>.
2. In the left sidebar, click **Projects** → **+ New project**.
3. Name: **AI Risk Assessment**
4. Description: *Conducts NIST-aligned risk assessments for AI and agentic systems before production approval.*

### 2. Set custom instructions

1. In the project, click **Set custom instructions** (or the project settings gear).
2. Paste the **entire content of `core/instructions.md`** (from the repo root).

Claude Projects has a generous instructions limit; the full ~7,200 characters fits comfortably with room to add organization-specific context (e.g., "When the system is a Microsoft 365 add-in, also reference our internal MS-365 threat-modeling addendum.")

### 3. Add project knowledge

Click **Add content** → **Upload from computer** and add all five files from `core/knowledge/`:

- `methodology.md`
- `security-checklist.md`
- `threat-library.md`
- `stride-reference.md`
- `output-template.md`

Claude reads markdown natively — no format conversion needed.

You can also optionally add:

- The example assessment in `assessments/GRC-News-Assistant-3-risk-assessment-2026-01-29.md` as a few-shot exemplar
- Your organization's specific threat-modeling templates
- The relevant NIST PDFs (SP 800-30 Rev 1, NIST IR 8596) if you want the agent to cite specific paragraphs

### 4. Enable connectors (optional)

If your Claude plan supports connectors (Pro/Max/Team), consider enabling:

- **Web search** — current OWASP / NIST updates
- **Google Drive** — pull architecture docs the analyst stores there

Skip Gmail and other PII-heavy connectors unless your engagement specifically requires them.

### 5. Test

Start a new chat in the project:

> "I need to assess a new internal chatbot that reads our HR knowledge base."

Verify the agent:

1. Asks about business purpose
2. Asks about architecture / data flows
3. Runs the Lethal Trifecta check
4. Recommends a tier
5. Cites at least one of the knowledge files (e.g., "per `methodology.md`...")

If any step is skipped, re-paste custom instructions.

### 6. Share (optional)

If on Claude Team:

1. Project settings → **Share** → invite teammates by email.
2. Decide whether other GRC analysts can edit knowledge or only chat.

## Maintenance

When `core/` content updates:

1. Re-paste `core/instructions.md` into Custom Instructions.
2. In Project Knowledge, delete the old version of any updated file and re-upload the new version.

## Why Claude for this work

- Long-context handling means Claude can read the full methodology + checklist + threat library + your architecture doc all at once without retrieval gaps.
- Markdown is native — no format conversion or rendering quirks.
- Project Knowledge persists across conversations, so an analyst can run multiple assessments in parallel chats without re-uploading context.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Agent doesn't reference uploaded files | Confirm files appear in Project Knowledge; click into a file to verify it parsed. |
| Trifecta check skipped | Custom instructions did not save — re-paste and click Save. |
| "I'm not sure about your methodology" | Knowledge files were not added. Re-upload all five from `core/knowledge/`. |
| Hits a context limit on long assessments | Start a new chat in the same project — knowledge persists, conversation context resets. |
