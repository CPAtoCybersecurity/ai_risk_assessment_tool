# Deploy on Microsoft 365 Copilot Agent Builder

> **Platform:** Microsoft 365 Copilot Agent Builder (the in-product, declarative agent experience inside M365 Copilot — *not* Microsoft Copilot Studio, which is a separate Power Platform product).
>
> **License required:** Microsoft 365 Copilot license. Org admin must have agent creation enabled for your tenant.
>
> **Time to install:** ~5 minutes.

## Where to find Agent Builder

Agent Builder lives **inside Microsoft 365 Copilot itself**:

- Web: <https://m365.cloud.microsoft/chat/> → click the **Agents** rail → **+ Create agent**
- Or: in Microsoft 365 Copilot Chat (Teams, Outlook, or web), click the agent icon → **Create agent**

If you only see Copilot Studio (copilotstudio.microsoft.com) — that's a different product. You want the streamlined builder *inside* M365 Copilot.

## Deployment Steps

### 1. Create the agent

1. Open Agent Builder via the path above.
2. Choose **Configure manually** (not "Describe").

### 2. Basic settings

| Field | Value |
|-------|-------|
| **Name** | AI Risk Assessment |
| **Description** | Conducts NIST-aligned risk assessments for AI and agentic systems before production approval. |
| **Icon** | Upload a shield or risk icon (your choice). |

### 3. Instructions

Paste the **entire content of `core/instructions.md`** (from the repo root) into the **Instructions** field.

The file is ~7,200 characters, well within Agent Builder's instruction limit.

### 4. Knowledge

Add knowledge sources. Upload all five files from `core/knowledge/`:

- `methodology.md`
- `security-checklist.md`
- `threat-library.md`
- `stride-reference.md`
- `output-template.md`

Agent Builder accepts `.md` as plain text. If you see ingest quality issues (e.g., tables not rendering well in retrieval), re-render the files to PDF locally and upload the PDFs instead.

You may optionally add a SharePoint site or Microsoft Graph connector if your org has internal threat-modeling references you want the agent to draw on.

### 5. Starter prompts

Paste the four starters from `core/starter-prompts.md`:

1. Create a risk assessment that is ready for CISO review.
2. I'm assessing a new AI system before production approval — start the intake.
3. Help me evaluate a third-party AI tool we're considering buying.
4. Recommend security controls for an AI feature I'm scoping.

### 6. Capabilities

- **Web search:** ON (lets the agent reference current OWASP / NIST publications)
- **Code interpreter:** OFF (not needed for assessment work)
- **Image generation:** OFF

### 7. Actions

None for the MVP. (A Phase-2 enhancement could add a Power Automate flow that posts the final assessment to a SharePoint list or ServiceNow — out of scope for the initial deploy.)

### 8. Test

In the Agent Builder preview pane, run:

- "I need to assess a new internal chatbot that reads our HR knowledge base."
- The agent should respond by asking about business purpose and architecture, then run the Lethal Trifecta check.

If it skips the trifecta or generates a report without intake, the instructions did not paste cleanly — re-paste from `core/instructions.md`.

### 9. Publish

1. Click **Publish**.
2. Choose the audience: just yourself (initial test), specific users, or your tenant.
3. After publish, the agent appears in the Agents rail of Microsoft 365 Copilot for the chosen audience.

## Maintenance

When `core/instructions.md` or any file in `core/knowledge/` is updated:

1. Open the agent in Agent Builder.
2. Replace the Instructions text with the new `core/instructions.md`.
3. In Knowledge, remove the old file and upload the new version.
4. Click **Update**.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Agent doesn't cite knowledge files | Confirm all 5 files uploaded; check tenant's knowledge-source settings. |
| Responses feel generic | Re-paste instructions; confirm full ~7K characters landed. |
| Trifecta check skipped | Instructions truncated on paste — paste again, watch for the trailing "Then proceed through INTAKE → TRIFECTA → …" line. |
| Agent won't publish | Org admin may have agent publishing disabled. Check with your M365 admin. |
