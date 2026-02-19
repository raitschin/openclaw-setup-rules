# OpenClaw Company Rules — GLXY / attractsoft

Company-wide rules for all OpenClaw agent instances. This repo is the **single source of truth** for how our AI agents work.

> **⚠️ Rule for Contributors:** When `openclaw-company-rules.yaml` is changed, this README **MUST** also be updated so all rules remain human-readable. No YAML update without a README update.

> **📦 What's NOT in here:** Anything OpenClaw already provides out of the box (memory management, heartbeat behavior, group chat etiquette, platform formatting, workspace file layout, reactions). This file only contains rules that go **beyond the defaults**.

---

## 📋 Table of Contents

- [Quick Start](#-quick-start)
- [Rules Overview](#-rules-overview)
  - [1. Deployment](#1-deployment-rules)
  - [2. Development Workflow (BMad)](#2-development-workflow-bmad-method)
  - [3. BMad Workflow Files](#3-bmad-workflow-files)
  - [4. Group Scopes](#4-group-scopes)
  - [5. Session Configuration](#5-session-configuration)
  - [6. Group Memory Persistence](#6-group-memory-persistence)
  - [7. Security Tooling](#7-security-tooling)
  - [8. GitHub Repositories](#8-github-repositories)
  - [9. Project-Specific Custom Instructions](#9-project-specific-custom-instructions)
  - [10. Instance Setup (Bootstrap)](#10-instance-setup-bootstrap)
- [Setup a New Instance](#-setup-a-new-instance)
- [Import / Export / Sync](#-import--export--sync)
- [Versioning](#-versioning)
- [Contributing](#-contributing)

---

## 🚀 Quick Start

### Set up a new instance (automated — recommended):
```
1. Create workspace directory
2. Copy openclaw-company-rules.yaml + BOOTSTRAP.md into the workspace
3. Start OpenClaw → agent reads BOOTSTRAP.md
4. Agent asks: "Who are you?" and "Which projects?"
5. Agent generates AGENTS.md, SOUL.md, USER.md, MEMORY.md, TOOLS.md
6. Done! 🎉
```

### Update an existing instance:
```
1. Pull latest YAML from GitHub
2. Tell your agent:
   "Reconcile your AGENTS.md with openclaw-company-rules.yaml.
    Show differences. Adopt new rules, keep personal customizations."
```

---

## 📖 Rules Overview

### 1. Deployment Rules

| ID | Rule | Severity |
|----|------|----------|
| **DEPLOY-001** | Production deployment requires explicit approval | 🔴 CRITICAL |
| **DEPLOY-002** | Staging first | 🔴 CRITICAL |
| **DEPLOY-003** | Verify deployments | 🟡 HIGH |

**DEPLOY-001** 🔴 — NEVER deploy to production without EXPLICIT approval. No push to `main`, no deploy hooks, no merges. **No exceptions.**

**DEPLOY-002** 🔴 — ALWAYS staging first. Only after successful test AND explicit owner OK → production.

**DEPLOY-003** 🟡 — GitHub webhooks don't always trigger. Manually verify deployments.

---

### 2. Development Workflow (BMad Method)

| ID | Rule | Severity |
|----|------|----------|
| **DEV-001** | No code without an issue | 🔴 CRITICAL |
| **DEV-002** | Use issue templates | 🟡 HIGH |
| **DEV-003** | BMad planning (top-down) | 🟡 HIGH |
| **DEV-004** | TDD — Red-Green-Refactor | 🔴 CRITICAL |
| **DEV-005** | Code review | 🟡 HIGH |
| **DEV-006** | Definition of Done | 🟡 HIGH |
| **DEV-007** | Commit messages reference issues | 🟡 HIGH |
| **DEV-008** | Issues in English | 🟡 HIGH |

**DEV-001** 🔴 — No code without a GitHub issue. ALL repos, ALL changes. Sub-agents too. **No exceptions.**

**DEV-002** — Use issue templates: Feature, Bug, or Change Request. Labels: `feature`, `bug`, `epic`, `marketing`.

**DEV-003** — Top-down: Epic → Stories → Tasks → Issues (with `ref #Epic`).

**DEV-004** 🔴 — TDD mandatory: 🔴 RED (tests fail) → 🟢 GREEN (minimal code) → 🔵 REFACTOR.

**DEV-005** — Code review before merge. Ideally with a different LLM. Check logic, edge cases, security. Offer browser testing for UI changes.

**DEV-006** — Definition of Done checklist: all tasks done, acceptance criteria met, tests green, file list complete, dev record documented, changelog updated.

**DEV-007** — Commits reference issues: `fixes #XX` or `ref #XX`.

**DEV-008** — All GitHub issues MUST be in English. Titles, descriptions, criteria, comments. No exceptions.

---

### 3. BMad Workflow Files

Paths relative to project root:

| File | Path |
|------|------|
| Workflow Engine | `_bmad/core/tasks/workflow.xml` |
| Dev-Story Workflow | `_bmad/bmm/workflows/4-implementation/dev-story/` |
| Config | `_bmad/bmm/config.yaml` |
| Story Template | `_bmad/bmm/workflows/4-implementation/create-story/template.md` |
| Story Output | `_bmad-output/implementation-artifacts/` |
| Code Review | `.claude/commands/bmad-bmm-code-review.md` |
| Quick Dev | `.claude/commands/bmad-agent-bmm-quick-flow-solo-dev.md` |

**3-Phase Workflow:**
1. **Planning** (agent): Create issue → create story file → update sprint
2. **Implementation** (sub-agent): BMad dev-story workflow + TDD
3. **Review** (agent): Code review → staging → owner test → production

---

### 4. Group Scopes

| ID | Rule | Severity |
|----|------|----------|
| **SCOPE-001** | Read scope before responding | 🔴 CRITICAL |
| **SCOPE-002** | Scope file structure | 🟡 HIGH |
| **SCOPE-003** | Adding new groups | 🟡 HIGH |

Every group has a scope file under `scopes/`. Agent reads it before responding. Out-of-scope requests are declined. No cross-project data sharing.

---

### 5. Session Configuration

| ID | Rule | Severity |
|----|------|----------|
| **SESSION-001** | Group session persistence (8h idle) | 🟡 HIGH |
| **SESSION-002** | Batch config changes | 🟠 MEDIUM |

Groups use 8h idle timeout instead of daily reset. Batch config changes to minimize restarts.

---

### 6. Group Memory Persistence

| ID | Rule | Severity |
|----|------|----------|
| **GMEM-001** | Every group gets a memory file | 🟡 HIGH |
| **GMEM-002** | Read memory on session start | 🔴 CRITICAL |
| **GMEM-003** | Update after meaningful exchanges | 🟡 HIGH |

File: `memory/{group-name}-context.md`. Read on session start, update after exchanges.

---

### 7. Security Tooling

| Tool | Type | Usage |
|------|------|-------|
| **Semgrep** | SAST | `semgrep --config auto <path>` |
| **Snyk** | SCA + SAST | `snyk test`, `snyk code test` |

**SEC-001** 🟡 — Security scan before every merge.
**SEC-002** 🟠 — Weekly full scans on all repos.
**SEC-003** 🟡 — Scan new dependencies before committing.

---

### 8. GitHub Repositories

**Organization:** `attractsoft`

| Repo | Description |
|------|-------------|
| `attractsoft/speaktomycrm` | Voice-to-CRM via Telegram |
| `attractsoft/florentin-app` | Automation workflows (SaaS enabler) |
| `attractsoft/anychat` | Chat frontend (anymize.ai) |
| `attractsoft/anymize` | GDPR-compliant LLM anonymization |
| `attractsoft/anymize-context` | Context repo: roadmap, pricing, BMad |

**Team:** Raitschin (`raitschin`), Nikolai (`Nraitschew`), Nick (`BurDeBug`)

**Labels:** `feature` · `bug` · `epic` · `marketing`

---

### 9. Project-Specific Custom Instructions

Each project can define custom instructions that **only apply to instances handling that project**. During setup, the agent asks which projects are relevant and only imports those.

| ID | Project | Repo | Description |
|----|---------|------|-------------|
| `speaktomycrm` | SpeakToMyCRM | `attractsoft/speaktomycrm` | Voice-to-CRM via Telegram |
| `florentin` | florentin.app | `attractsoft/florentin-app` | Automation workflows (SaaS enabler) |
| `anymize` | anymize.ai | `attractsoft/anymize` | GDPR-compliant LLM anonymization |
| `anyclaw` | AnyClaw | `attractsoft/anyclaw` | Browser-based AI assistant + GDPR |
| `anychat` | AnyChat | `attractsoft/anychat` | Chat frontend (anymize.ai) |
| `edaira` | edaira.ai | — | AI school assignment correction |
| `peuka` | peuka.com | — | Airbnb for parking spots |

**Custom instructions are initially empty** — each team fills them in as they work on their project. The structure is ready; the content grows organically.

**To add project-specific rules:**
1. Edit `openclaw-company-rules.yaml` → `projects:` section
2. Add `custom_instructions` content for your project
3. Update this README
4. Bump version, commit, push

---

### 10. Instance Setup (Bootstrap)

New instances auto-configure using `BOOTSTRAP.md`:

1. Agent reads `openclaw-company-rules.yaml`
2. Asks: **Who is the owner?** (name, role, timezone)
3. Asks: **Which projects?** (shows list, multiple selection)
4. Generates workspace files:
   - `AGENTS.md` — Global rules + selected project custom instructions
   - `SOUL.md` — Agent personality
   - `USER.md` — Owner info
   - `MEMORY.md` — Empty long-term memory
   - `TOOLS.md` — Empty local notes
5. Deletes `BOOTSTRAP.md`

**Files in this repo:**
- `openclaw-company-rules.yaml` — The rules (machine-readable)
- `BOOTSTRAP.md` — Copy to new workspace for auto-setup
- `README.md` — This file (human-readable documentation)

---

## 🔄 Import / Export / Sync

### Import (new instance — automated)
```
1. Copy openclaw-company-rules.yaml + BOOTSTRAP.md to workspace
2. Start OpenClaw → agent auto-configures
```

### Import (new instance — manual)
```
1. Copy openclaw-company-rules.yaml to workspace
2. Tell agent: "Import company rules. I work on projects: X, Y, Z."
3. Agent generates AGENTS.md with relevant rules
```

### Export (existing instance)
```
1. Tell agent: "Export company rules to openclaw-company-rules.yaml. No personal data."
2. Agent updates YAML with new version
```

### Sync (reconcile)
```
1. Pull latest YAML
2. Tell agent: "Reconcile AGENTS.md with openclaw-company-rules.yaml."
3. Agent shows diff, adopts new rules, keeps customizations
```

### Add a project
```
1. Add entry under `projects:` in YAML
2. Fill in custom_instructions (or leave empty for now)
3. Update README
4. Bump version, commit, push
5. All instances can pick up the new project on next sync
```

---

## 📌 Versioning

Schema: `MAJOR.MINOR.PATCH`

| Type | When |
|------|------|
| **Major** | Breaking changes — rules removed or fundamentally changed |
| **Minor** | New rules or sections added |
| **Patch** | Clarifications, wording improvements |

**Current version:** `3.0.0` (2026-02-19)

### Changelog

- **3.0.0** (2026-02-19): Added Project-Specific Custom Instructions (Section 9) and Bootstrap Setup (Section 10). Added BOOTSTRAP.md for automated instance setup. Added DEV-008 (issues in English). Added team members to repositories section. New setup flow: one rules file manages all instances + projects.
- **2.1.0** (2026-02-18): Added Session Configuration (SESSION-001, SESSION-002).
- **2.0.0** (2026-02-18): Complete rewrite in English. Added Group Scopes. Removed OpenClaw defaults.
- **1.2.0** (2026-02-18): Initial version with Security Tooling.

---

## 🤝 Contributing

1. Develop and test the rule in your own instance
2. Export to `openclaw-company-rules.yaml`
3. Create a PR with:
   - YAML change
   - README update (**MANDATORY!**)
   - Version bump
4. Review + merge
5. All instances sync

> **Golden Rule:** No YAML update without a README update. If it's not documented here, it doesn't exist.
