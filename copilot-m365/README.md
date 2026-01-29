# AI Risk Assessment Agent - Copilot M365 Deployment

## Overview

This folder contains everything needed to deploy the AI Risk Assessment Agent in Microsoft Copilot Studio for M365.

## Contents

```
copilot-m365/
├── README.md                    # This file
├── system-prompt.md             # Agent configuration and system prompt
└── knowledge-files/
    ├── methodology.txt           # Risk framework (condensed)
    ├── security-checklist.txt    # Controls checklist (condensed)
    ├── stride-reference.txt      # STRIDE quick reference
    ├── threat-library.txt        # Common threats
    └── output-template.txt       # Report format
```

## Deployment Steps

### 1. Access Copilot Studio

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Sign in with your M365 account
3. Select your environment

### 2. Create New Agent

1. Click **Create** → **New agent**
2. Select **Configure manually**

### 3. Configure Basic Settings

| Field | Value |
|-------|-------|
| **Name** | AI Risk Assessment Agent |
| **Description** | Conducts structured risk assessments for AI and agentic systems following NIST 800-30 methodology |
| **Icon** | Upload a shield or risk-related icon |

### 4. Add Instructions

1. Open `system-prompt.md`
2. Copy the content inside the code block under "System Prompt"
3. Paste into the **Instructions** field in Copilot Studio

### 5. Upload Knowledge Files

1. In Copilot Studio, go to **Knowledge** section
2. Click **Add knowledge** → **Files**
3. Upload all 5 files from the `knowledge-files/` folder:
   - `methodology.txt`
   - `security-checklist.txt`
   - `stride-reference.txt`
   - `threat-library.txt`
   - `output-template.txt`

### 6. Configure Conversation Starters

Add these suggested prompts:
- "I need to assess a new AI chatbot"
- "Help me evaluate an agentic automation"
- "Run a Lethal Trifecta check on my system"
- "Generate a risk assessment report"

### 7. Test the Agent

1. Use the **Test** panel in Copilot Studio
2. Try these test scenarios:
   - Start a new assessment
   - Ask about the Lethal Trifecta
   - Request a threat analysis
   - Generate a sample report

### 8. Publish

1. Click **Publish**
2. Choose distribution channel (Teams, Web, etc.)
3. Configure access permissions

## Usage Guide

### Starting an Assessment

Users can begin by saying:
- "I need to assess [system name]"
- "Help me evaluate our new AI tool"
- "Start a risk assessment"

### Key Commands

| Say This | Agent Does |
|----------|------------|
| "Check the Lethal Trifecta" | Assesses the three critical risk factors |
| "What tier should this be?" | Recommends assessment tier |
| "Analyze threats" | Runs STRIDE threat modeling |
| "Walk through the checklist" | Goes through security controls |
| "Generate the report" | Creates assessment document |

### Assessment Flow

```
1. Intake → 2. Trifecta → 3. Tier → 4. Threats → 5. Checklist → 6. Report
```

## Comparison with Claude Code Version

| Aspect | Copilot M365 | Claude Code |
|--------|--------------|-------------|
| **Orchestration** | Agent manages flow | User invokes skills |
| **Knowledge** | Attached files | Direct file reading |
| **Depth** | Bounded by context | Can go deeper on demand |
| **Flexibility** | Fixed workflow | Mix/match skills |
| **Output** | Conversation + copy | Writes to local files |

## Maintenance

### Updating Knowledge Files

1. Edit files in `knowledge-files/`
2. In Copilot Studio, go to **Knowledge**
3. Delete old versions
4. Upload updated files

### Updating Instructions

1. Edit `system-prompt.md`
2. Copy new system prompt
3. Paste into Copilot Studio Instructions

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Agent doesn't find knowledge | Verify files uploaded, check file format |
| Responses too generic | Make instructions more specific |
| Missing methodology steps | Ensure methodology.txt is uploaded |
| Context limit errors | Condense knowledge files further |

## Support

For issues with:
- **Copilot Studio**: [Microsoft Documentation](https://learn.microsoft.com/copilot-studio/)
- **Methodology**: Refer to NIST SP 800-30
- **AI Threats**: Refer to OWASP Top 10 for LLM/Agentic Applications
