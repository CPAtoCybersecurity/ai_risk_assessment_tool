# Deploy as a Custom GPT

> **Platform:** Custom GPT in ChatGPT.
>
> **License required:** ChatGPT Plus, Team, Enterprise, or Edu (Custom GPT creation).
>
> **Time to install:** ~5 minutes.

## Deployment Steps

### 1. Open the GPT editor

1. Go to <https://chatgpt.com/gpts/editor>.
2. Click **Create** in the upper right.
3. Switch to the **Configure** tab (skip the conversational "Create" wizard).

### 2. Basic settings

| Field | Value |
|-------|-------|
| **Name** | AI Risk Assessment |
| **Description** | NIST-aligned risk assessments for AI and agentic systems. |
| **Profile picture** | Generate a shield or risk icon, or upload your own. |

### 3. Instructions

Paste the **entire content of `core/instructions.md`** (from the repo root) into the **Instructions** field.

ChatGPT's instructions field caps at 8,000 characters. The canonical instructions are ~7,200 characters — fits with headroom for organization-specific additions.

### 4. Conversation starters

Add these four prompts (Custom GPT shows up to 4 on launch):

1. Create a risk assessment that is ready for CISO review.
2. I'm assessing a new AI system before production approval — start the intake.
3. Help me evaluate a third-party AI tool we're considering buying.
4. Recommend security controls for an AI feature I'm scoping.

### 5. Knowledge

In the **Knowledge** section, click **Upload files** and add all five markdown files from `core/knowledge/`:

- `methodology.md`
- `security-checklist.md`
- `threat-library.md`
- `stride-reference.md`
- `output-template.md`

ChatGPT handles markdown well. Up to 20 files / 512 MB each / 2 M tokens per file — far more than this project needs.

### 6. Capabilities

- **Web Browsing:** ✅ ON (current OWASP / NIST references)
- **DALL·E Image Generation:** ❌ OFF
- **Code Interpreter & Data Analysis:** ❌ OFF (not needed for assessment work)

### 7. Actions

None in the MVP.

If you want to extend later: Custom GPT supports OpenAPI 3.0 actions. Phase-2 candidates:

- Post final assessment to a ServiceNow GRC table
- Open a Jira ticket for each remediation action
- Write the assessment to a SharePoint list

These require an OpenAPI spec and an authentication mechanism — out of scope for the initial deployment.

### 8. Test

In the editor's **Preview** pane on the right, run:

> "I need to assess a new internal chatbot that reads our HR knowledge base."

Verify intake-first behavior, then trifecta check, then tier selection. If the agent jumps straight to a report, the instructions did not paste cleanly.

### 9. Save and share

1. Click **Create** (top right).
2. Choose visibility:
   - **Only me** — private use
   - **Anyone with the link** — internal sharing within your org
   - **GPT Store** — public listing (requires verified builder profile)

For internal GRC use, **"Anyone with the link"** is the right default.

## Maintenance

When `core/` content updates:

1. Open the GPT in the editor.
2. Replace the Instructions text with the new `core/instructions.md`.
3. In Knowledge, remove the old file version and upload the new one.
4. Click **Update**.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Instructions truncated on paste | Confirm under 8,000 characters with `wc -c core/instructions.md`. |
| Agent doesn't cite uploaded files | Knowledge files weren't added — confirm all five appear in the Knowledge section. |
| Trifecta check skipped | Instructions did not save fully — paste again, save, retest. |
| Conversation starters missing | They live in **Configure** → scroll down past Instructions. |
