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
  - [6. Security Tooling](#6-security-tooling)
  - [7. GitHub Repositories](#7-github-repositories)
- [Import / Export / Sync](#-import--export--sync)
- [Versioning](#-versioning)
- [Contributing](#-contributing)

---

## 🚀 Quick Start

**Set up a new instance:**
```
1. Clone this repo
2. Instruct the agent:
   "Import the company rules from openclaw-company-rules.yaml
    into your AGENTS.md. Personal sections (SOUL.md, USER.md)
    remain individual."
3. The agent merges the rules into its AGENTS.md
```

**Update an existing instance:**
```
1. Pull latest YAML from GitHub
2. Instruct the agent:
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

**DEPLOY-001 — Production deployment requires explicit approval**
> NEVER deploy to production without EXPLICIT approval from the responsible owner.
> - No push to `main` branch without approval
> - No production deploy hook without approval
> - No merge to `main` without approval
> - **No exceptions. Not even "it's just a small fix". NONE.**

**DEPLOY-002 — Staging first**
> ALWAYS work on and deploy to `staging` first. Only after successful testing on staging AND explicit confirmation from the owner may production be deployed.
>
> Workflow: `staging branch → test → PR/merge to main → production`

**DEPLOY-003 — Verify deployments**
> GitHub webhooks don't always trigger automatically. Manually trigger deploy hooks when needed and verify the deployment succeeded.

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

**DEV-001 — No code without an issue** 🔴
> Not a single line of code may be written before a GitHub issue exists. This applies to ALL repos, ALL changes — features, bugfixes, refactoring, cleanup. Sub-agents must also have an issue before they implement anything. **No exceptions.**

**DEV-002 — Use issue templates**
> Every issue MUST be created from a template:
> - Feature: `.github/ISSUE_TEMPLATE/feature.md`
> - Bug: `.github/ISSUE_TEMPLATE/bug.md`
> - Change Request: `.github/ISSUE_TEMPLATE/change-request.md`
>
> Labels: `feature`, `bug`, `epic`, `marketing`

**DEV-003 — BMad planning (top-down)**
> 1. **Epic** → contains all stories
> 2. **Stories** from Epic → clear user stories with acceptance criteria
> 3. **Tasks** per Story → concrete technical tasks
> 4. **Issues** per Task → from templates, with `ref #Epic`

**DEV-004 — TDD (Test-Driven Development)** 🔴
> - 🔴 **RED**: Write tests FIRST — tests must FAIL
> - 🟢 **GREEN**: Write minimal code until all tests pass
> - 🔵 **REFACTOR**: Improve code structure, tests must stay green
>
> Commits reference the issue (`fixes #123`, `ref #123`)

**DEV-005 — Code review**
> - Code review is mandatory before merge
> - Check: logic, edge cases, security, performance
> - Run all tests — no regressions
> - For UI/frontend changes: offer browser testing
> - Ideally: review with a DIFFERENT LLM than the one that implemented

**DEV-006 — Definition of Done (per story)**
> - [ ] All tasks/subtasks checked off
> - [ ] All acceptance criteria met
> - [ ] Unit tests for core functionality
> - [ ] Integration tests where needed
> - [ ] All tests green (no regressions)
> - [ ] File list complete
> - [ ] Dev agent record documented
> - [ ] Change log updated

**DEV-007 — Commit messages**
> Every commit MUST reference its associated issue:
> - `fixes #XX` — when the commit closes the issue
> - `ref #XX` — when the commit contributes to the issue

---

### 3. BMad Workflow Files

Paths are relative to the project root:

| File | Path |
|------|------|
| Workflow Engine | `_bmad/core/tasks/workflow.xml` |
| Dev-Story Workflow | `_bmad/bmm/workflows/4-implementation/dev-story/` |
| Config | `_bmad/bmm/config.yaml` |
| Story Template | `_bmad/bmm/workflows/4-implementation/create-story/template.md` |
| Story Output | `_bmad-output/implementation-artifacts/` |
| Code Review | `.claude/commands/bmad-bmm-code-review.md` |
| Quick Dev | `.claude/commands/bmad-agent-bmm-quick-flow-solo-dev.md` |

**3-Phase Workflow for Sub-Agent Spawns:**

```
Phase 1 (Planning — agent itself):
  → Create GitHub issue (from template)
  → Create story file
  → Update sprint status

Phase 2 (Implementation — sub-agent):
  → Follow BMad dev-story workflow autonomously
  → TDD: Red → Green → Refactor
  → Check off story file checklist

Phase 3 (Review — agent itself):
  → Code review (ideally with a different LLM)
  → Deploy to staging + test
  → Ask owner to test
  → After OK: merge to main → production
```

---

### 4. Group Scopes

| ID | Rule | Severity |
|----|------|----------|
| **SCOPE-001** | Read scope before responding | 🔴 CRITICAL |
| **SCOPE-002** | Scope file structure | 🟡 HIGH |
| **SCOPE-003** | Adding new groups | 🟡 HIGH |

**SCOPE-001 — Read scope before responding** 🔴
> Every group chat has a scope file under `scopes/`. Before answering in ANY group:
> 1. Read the scope file for that group's Chat-ID
> 2. Check if the request falls within the allowed scope
> 3. If NOT in scope → politely decline and redirect to the correct group
> 4. NEVER share data from other projects or private user context

**SCOPE-002 — Scope file structure**
> Each scope file must define:
> - **Chat-ID**: the group identifier
> - **Purpose**: what the group is for (one sentence)
> - **Allowed**: list of permitted activities and repos
> - **Forbidden**: list of explicitly prohibited activities
> - **Context Files**: paths the agent may reference for this group

**SCOPE-003 — Adding new groups**
> When creating a new group:
> 1. Create scope file: `scopes/group-{project}-{purpose}.md`
> 2. Add Chat-ID to OpenClaw config (`channels.telegram.groups`)
> 3. Add reference to AGENTS.md group scopes section
> 4. Commit scope file to the company rules repo

**Example scope file:**
```markdown
# Group Scope: Project X — Marketing

## Chat-ID: -100XXXXXXXXXX
## Purpose: Marketing content and strategy for Project X

## Allowed:
- Marketing copy, landing pages, blog posts for Project X
- Social media content for Project X
- Repo: org/project-x (website content only)

## Forbidden:
- Other projects
- Personal tasks, MEMORY.md access
- Backend code changes or deployments

## Context Files:
- projects/project-x/website/
```

---

### 5. Session Configuration

| ID | Rule | Severity |
|----|------|----------|
| **SESSION-001** | Group session persistence (8h idle) | 🟡 HIGH |
| **SESSION-002** | Avoid unnecessary gateway restarts | 🟠 MEDIUM |

**SESSION-001 — Group session persistence** 🟡
> Group chat sessions use idle-based reset instead of daily reset. Default: **480 minutes (8 hours)** idle timeout. This prevents group context loss during gateway restarts (config patches, updates, etc.) which would otherwise create a new session ID and lose all prior conversation context.

**Config:**
```json
{
  "session": {
    "resetByType": {
      "group": { "mode": "idle", "idleMinutes": 480 }
    }
  }
}
```

**SESSION-002 — Avoid unnecessary gateway restarts**
> Every gateway restart re-evaluates session expiry. Batch config changes together when possible to minimize restarts and reduce the risk of session context loss in groups.

---

### 6. Security Tooling

| Tool | Type | Install | Usage |
|------|------|---------|-------|
| **Semgrep** | SAST | `pip install semgrep` | `semgrep --config "p/typescript" <path>` |
| **Snyk** | SCA + SAST | `npm install -g snyk` | `snyk test`, `snyk code test` |

| ID | Rule | Severity |
|----|------|----------|
| **SEC-001** | Security scan before merge | 🟡 HIGH |
| **SEC-002** | Regular scans (weekly) | 🟠 MEDIUM |
| **SEC-003** | Scan new dependencies | 🟡 HIGH |

**SEC-001 — Security scan before merge**
> Before every merge to staging or main:
> - Semgrep SAST scan (`--config auto` or `p/typescript`)
> - Snyk dependency scan (`snyk test`) — no critical/high vulnerabilities
> - Blocking findings must be resolved

**SEC-002 — Regular scans**
> At least weekly: `snyk monitor` + Semgrep full scan on all active repos.

**SEC-003 — New dependencies**
> For every new dependency: `snyk test` before committing. Check license compatibility.

**Semgrep Rulesets:**
`p/typescript` · `p/javascript` · `p/react` · `p/nodejs` · `p/owasp-top-ten` · `p/security-audit`

---

### 7. GitHub Repositories

**Organization:** `attractsoft`

| Repo | Description |
|------|-------------|
| `attractsoft/speaktomycrm` | Voice-to-CRM via Telegram |
| `attractsoft/florentin-app` | Automation workflows to API products (SaaS enabler) |
| `attractsoft/anychat` | Chat frontend (part of anymize.ai) |
| `attractsoft/anymize` | GDPR-compliant LLM anonymization |
| `attractsoft/anymize-context` | Context repo: roadmap, pricing, BMad workflow |

**Labels:** `feature` · `bug` · `epic` · `marketing`

---

## 🔄 Import / Export / Sync

### Import (new instance)
```
1. Copy openclaw-company-rules.yaml to the workspace
2. Instruct the agent:
   "Import the company rules from openclaw-company-rules.yaml
    into your AGENTS.md. Personal sections remain individual."
3. The agent merges the rules into its AGENTS.md
```

### Export (existing instance)
```
1. Instruct the agent:
   "Export your current company rules from AGENTS.md
    to openclaw-company-rules.yaml format. No personal data."
2. The agent updates openclaw-company-rules.yaml with a new version
```

### Sync (reconcile)
```
1. Pull current version from GitHub
2. Instruct the agent:
   "Reconcile your AGENTS.md with openclaw-company-rules.yaml.
    Show differences. Adopt new rules, keep personal customizations."
3. Bump version + commit
```

---

## 📌 Versioning

Schema: `MAJOR.MINOR.PATCH`

| Type | When |
|------|------|
| **Major** | Breaking changes — rules removed or fundamentally changed |
| **Minor** | New rules added |
| **Patch** | Clarifications, wording improvements |

**Current version:** `2.1.0` (2026-02-18)

### Changelog

- **2.1.0** (2026-02-18): Added Session Configuration (SESSION-001, SESSION-002) — group sessions now use 8h idle timeout to survive gateway restarts.
- **2.0.0** (2026-02-18): Complete rewrite in English. Added Group Scopes (SCOPE-001 to 003). Removed OpenClaw defaults (memory, heartbeats, group chat etiquette, formatting, workspace layout). Streamlined to company-specific rules only.
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
