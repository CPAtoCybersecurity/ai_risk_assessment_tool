# Deploy as a Gemini Gem

> **Platform:** Custom Gem in Gemini.
>
> **License required:** Google AI Pro, Google AI Ultra, or Google Workspace with Gemini.
>
> **Time to install:** ~5 minutes.

## Deployment Steps

### 1. Open the Gem editor

1. Go to <https://gemini.google.com/gems/create>.
2. Or in Gemini chat → click the Gem icon (left sidebar) → **+ New Gem**.

### 2. Basic settings

| Field | Value |
|-------|-------|
| **Name** | AI Risk Assessment Generator |
| **Description** | NIST-aligned risk assessments for AI and agentic systems. |

### 3. Instructions

Paste the **entire content of `core/instructions.md`** (from the repo root) into the **Instructions** field.

The full ~7,200 characters fits within Gemini's instructions limit.

### 4. Knowledge files

In the Gem editor, click **Knowledge** → **+ Upload files** and add the five markdown files from `core/knowledge/`:

- `methodology.md`
- `security-checklist.md`
- `threat-library.md`
- `stride-reference.md`
- `output-template.md`

If your Gemini tier doesn't yet expose knowledge file uploads (this feature has been rolling out unevenly — check your account), use this fallback:

> Append the contents of all five knowledge files **inline** at the end of the Instructions field, each preceded by a clear delimiter such as `--- methodology.md ---`. Gemini handles long context well; the inline approach works reliably.

### 5. Capabilities

- **Web access / search** — ON if available in your Gem settings.
- **Other tools** — leave default.

### 6. Test

Click **Preview** and try:

> "I need to assess a new internal chatbot that reads our HR knowledge base."

Verify the agent runs intake first, then the Lethal Trifecta, then tier selection. If it skips ahead to a report, the instructions did not paste cleanly.

### 7. Save and use

1. Click **Save**.
2. The Gem appears under **Your Gems** in the Gemini sidebar.

## Sharing

If on Google Workspace with Gemini, you can share Gems within your organization:

1. Open the Gem → **Share** → choose audience.
2. Workspace admins control whether Gems can be shared cross-org.

## Maintenance

When `core/` content updates:

1. Open the Gem in the editor.
2. Replace the Instructions field with the latest `core/instructions.md` (or fallback inline blocks).
3. In Knowledge, remove and re-upload changed files.
4. Save.

## Notes on Gemini-specific behavior

- Gemini tends to be slightly more verbose than Claude or GPT in default tone. If the agent's responses run long, append this line to the Instructions: *"Be concise. Default to short, scannable answers; expand only when explicitly asked."*
- Gemini's knowledge-file ingestion has historically lagged Claude and ChatGPT. If you see retrieval gaps (the agent answers as if it didn't read the files), prefer the **inline-knowledge fallback** described in step 4.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Knowledge upload not available | Use the inline fallback (append knowledge to Instructions). |
| Agent ignores knowledge files | Switch to inline fallback or reduce knowledge to 3 most-relevant files. |
| Trifecta check skipped | Re-paste Instructions; verify the trailing "Then proceed through…" line is present. |
| Verbose, hedging responses | Append the conciseness directive (see notes above). |
