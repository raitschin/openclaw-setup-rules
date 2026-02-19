# BOOTSTRAP.md — OpenClaw Instance Setup

Welcome! This is a new OpenClaw instance for **GLXY / attractsoft**.

## Your first task

You are being set up as a new agent instance. Follow these steps:

### Step 1: Read company rules
Read `openclaw-company-rules.yaml` in this workspace. This contains all company-wide rules and project definitions.

### Step 2: Ask who the owner is
Ask the user:
- **Name** (who are you?)
- **Role** (what do you do?)
- **Timezone** (e.g. Europe/Berlin)
- **Preferred language** (Deutsch/English)

### Step 3: Ask which projects
Show the user the available projects from the YAML file's `projects:` section. Format as a numbered list:

```
Which projects does this instance handle? (comma-separated numbers)

1. SpeakToMyCRM — Voice-to-CRM via Telegram
2. florentin.app — Automation workflows to API products
3. anymize.ai — GDPR-compliant LLM anonymization
4. AnyClaw — Browser-based AI assistant with GDPR anonymization
5. AnyChat — Chat frontend (part of anymize.ai)
6. edaira.ai — AI school assignment correction
7. peuka.com — Airbnb for parking spots
```

The user can select one or more.

### Step 4: Generate workspace files

**AGENTS.md** — Combine:
- All global rules (Sections 1-8 from the YAML)
- Custom instructions ONLY for the selected projects (Section 9)
- Skip custom instructions for non-selected projects entirely

**SOUL.md** — Ask the user:
- "What should this agent's name be?"
- "Any specific personality traits or communication style?"
- Or offer a default: friendly, direct, hands-on, German-speaking

**USER.md** — From the owner info in Step 2

**MEMORY.md** — Create with header:
```markdown
# MEMORY.md — [Agent Name]'s Long-Term Memory
## Setup
- Created: [today's date]
- Owner: [name]
- Projects: [selected projects]
```

**TOOLS.md** — Create with header:
```markdown
# TOOLS.md — Local Notes
```

### Step 5: Clean up
- Delete this BOOTSTRAP.md file (you won't need it again)
- Summarize what was set up:
  - Agent name
  - Owner
  - Selected projects
  - Files created

### Important notes
- Global rules apply to ALL instances — never skip them
- Project custom instructions are ONLY for selected projects
- Personal data (USER.md, SOUL.md) is instance-specific and NOT shared
- When in doubt about a rule, refer back to openclaw-company-rules.yaml
