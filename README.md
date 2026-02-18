# OpenClaw Company Rules — GLXY / attractsoft

Firmenweite Regeln für alle OpenClaw-Instanzen. Dieses Repo ist die **Single Source of Truth** für unsere AI-Agent-Arbeitsweise.

> **⚠️ Regel für Contributors:** Wenn `openclaw-company-rules.yaml` geändert wird, **MUSS** diese README ebenfalls aktualisiert werden, damit alle Regeln auch human-readable nachvollziehbar bleiben. Kein YAML-Update ohne README-Update.

---

## 📋 Inhaltsverzeichnis

- [Schnellstart](#-schnellstart)
- [Regeln im Überblick](#-regeln-im-überblick)
  - [1. Deployment](#1-deployment-rules)
  - [2. Development Workflow (BMad)](#2-development-workflow-bmad-methode)
  - [3. BMad Workflow-Dateien](#3-bmad-workflow-dateien)
  - [4. Safety & Permissions](#4-safety--permissions)
  - [5. Group Chat Behavior](#5-group-chat-behavior)
  - [6. Memory System](#6-memory-system)
  - [7. Workspace Structure](#7-workspace-structure)
  - [8. Heartbeats & Proactive Work](#8-heartbeats--proaktives-arbeiten)
  - [9. Platform Formatting](#9-platform-formatting)
  - [10. Security Tooling](#10-security-tooling)
  - [11. GitHub Repositories](#11-github-repositories)
  - [12. Communication](#12-kommunikation)
- [Import / Export / Sync](#-import--export--sync)
- [Versionierung](#-versionierung)
- [Contributing](#-contributing)

---

## 🚀 Schnellstart

**Neue Instanz aufsetzen:**
```
1. Repo klonen
2. Agent instruieren:
   "Importiere die Company Rules aus openclaw-company-rules.yaml
    in deine AGENTS.md. Persönliche Sections (SOUL.md, USER.md)
    bleiben individuell."
3. Agent merged die Regeln in seine AGENTS.md
```

**Bestehende Instanz updaten:**
```
1. Aktuelles YAML von GitHub pullen
2. Agent instruieren:
   "Gleiche deine AGENTS.md mit openclaw-company-rules.yaml ab.
    Zeige Differenzen. Übernimm neue Regeln, behalte persönliche Anpassungen."
```

---

## 📖 Regeln im Überblick

### 1. Deployment Rules

| ID | Regel | Severity |
|----|-------|----------|
| **DEPLOY-001** | Production Deployment erfordert explizite Freigabe | 🔴 CRITICAL |
| **DEPLOY-002** | Staging First | 🔴 CRITICAL |
| **DEPLOY-003** | Deploy Hooks verifizieren | 🟡 HIGH |

**DEPLOY-001 — Production Deployment erfordert explizite Freigabe**
> NIEMALS auf Production deployen ohne AUSDRÜCKLICHE Freigabe des zuständigen Owners.
> - Kein Push auf `main` Branch ohne Freigabe
> - Kein Production Deploy Hook triggern ohne Freigabe
> - Kein `merge` nach `main` ohne Freigabe
> - **Keine Ausnahmen. Keine "ist ja nur ein kleiner Fix". KEINE.**

**DEPLOY-002 — Staging First**
> IMMER auf `staging` arbeiten und deployen. Erst nach erfolgreichem Test auf Staging UND ausdrücklicher Bestätigung des Owners darf nach Production deployed werden.
>
> Workflow: `staging Branch → testen → PR/merge nach main → production`

**DEPLOY-003 — Deploy Hooks verifizieren**
> GitHub Webhooks funktionieren nicht immer automatisch. Deploy Hooks bei Bedarf manuell triggern und Deployment verifizieren.

---

### 2. Development Workflow (BMad-Methode)

| ID | Regel | Severity |
|----|-------|----------|
| **DEV-001** | Kein Code ohne Issue | 🔴 CRITICAL |
| **DEV-002** | Issue-Templates verwenden | 🟡 HIGH |
| **DEV-003** | BMad Planung (Top-Down) | 🟡 HIGH |
| **DEV-004** | TDD — Red-Green-Refactor | 🔴 CRITICAL |
| **DEV-005** | Code Review | 🟡 HIGH |
| **DEV-006** | Definition of Done | 🟡 HIGH |
| **DEV-007** | Commit-Messages mit Issue-Ref | 🟡 HIGH |

**DEV-001 — Kein Code ohne Issue** 🔴
> Keine einzige Zeile Code darf geschrieben werden, bevor ein GitHub Issue existiert. Das gilt für ALLE Repos, ALLE Änderungen — Features, Bugfixes, Refactoring, Cleanup. Auch Subagents müssen zuerst ein Issue haben. **Keine Ausnahmen.**

**DEV-002 — Issue-Templates verwenden**
> Jedes Issue MUSS aus einem Template erstellt werden:
> - Feature: `.github/ISSUE_TEMPLATE/feature.md`
> - Bug: `.github/ISSUE_TEMPLATE/bug.md`
> - Change Request: `.github/ISSUE_TEMPLATE/change-request.md`
>
> Labels: `feature`, `bug`, `epic`, `marketing`

**DEV-003 — BMad Planung (Top-Down)**
> 1. **Epic** erstellen → enthält alle Stories
> 2. **Stories** aus Epic ableiten → klare User Stories mit Akzeptanzkriterien
> 3. **Tasks** pro Story definieren → konkrete technische Aufgaben
> 4. **Issues** pro Task erstellen → aus Templates, mit `ref #Epic`

**DEV-004 — TDD (Test-Driven Development)** 🔴
> - 🔴 **RED**: Tests ZUERST schreiben — Tests müssen FEHLSCHLAGEN
> - 🟢 **GREEN**: Minimalen Code implementieren bis alle Tests grün sind
> - 🔵 **REFACTOR**: Code-Struktur verbessern, Tests müssen grün bleiben
>
> Commits referenzieren das Issue (`fixes #123`, `ref #123`)

**DEV-005 — Code Review**
> - Code Review ist Pflicht vor Merge
> - Prüfung: Logik, Edge Cases, Security, Performance
> - Alle Tests laufen lassen — keine Regressionen
> - Bei UI/Frontend-Änderungen: Browser-Test anbieten
> - Idealerweise: Review mit einem ANDEREN LLM als dem Implementierer

**DEV-006 — Definition of Done (pro Story)**
> - [ ] Alle Tasks/Subtasks abgehakt
> - [ ] Alle Acceptance Criteria erfüllt
> - [ ] Unit Tests für Kernfunktionalität
> - [ ] Integration Tests wo nötig
> - [ ] Alle Tests grün (keine Regressionen)
> - [ ] File List vollständig
> - [ ] Dev Agent Record dokumentiert
> - [ ] Change Log aktualisiert

**DEV-007 — Commit-Messages**
> Jeder Commit MUSS das zugehörige Issue referenzieren:
> - `fixes #XX` — wenn der Commit das Issue schließt
> - `ref #XX` — wenn der Commit zum Issue beiträgt

---

### 3. BMad Workflow-Dateien

Dateipfade im Repo (relativ zum Projekt-Root):

| Datei | Pfad |
|-------|------|
| Workflow Engine | `_bmad/core/tasks/workflow.xml` |
| Dev-Story Workflow | `_bmad/bmm/workflows/4-implementation/dev-story/` |
| Config | `_bmad/bmm/config.yaml` |
| Story Template | `_bmad/bmm/workflows/4-implementation/create-story/template.md` |
| Story Output | `_bmad-output/implementation-artifacts/` |
| Code Review | `.claude/commands/bmad-bmm-code-review.md` |
| Quick Dev | `.claude/commands/bmad-agent-bmm-quick-flow-solo-dev.md` |

**3-Phasen-Workflow für Subagent-Spawns:**

```
Phase 1 (Planung — Agent selbst):
  → GitHub Issue erstellen (aus Template)
  → Story-File erstellen
  → Sprint-Status updaten

Phase 2 (Implementierung — Subagent):
  → BMad dev-story Workflow autonom folgen
  → TDD: Red → Green → Refactor
  → Story-File Checkliste abhaken

Phase 3 (Review — Agent selbst):
  → Code Review (idealerweise anderes LLM)
  → Staging deployen + testen
  → Owner zum Test auffordern
  → Nach OK: merge nach main → production
```

---

### 4. Safety & Permissions

| ID | Regel | Severity |
|----|-------|----------|
| **SAFE-001** | Keine Daten-Exfiltration | 🔴 CRITICAL |
| **SAFE-002** | `trash` > `rm` | 🟡 HIGH |
| **SAFE-003** | Destruktive Commands erst bestätigen lassen | 🟡 HIGH |
| **SAFE-004** | Externe Aktionen erst absprechen | 🟡 HIGH |
| **SAFE-005** | MEMORY.md nur in Main Session | 🔴 CRITICAL |

**SAFE-001** — Private Daten niemals nach außen geben. Keine Ausnahmen.

**SAFE-002** — Wiederherstellbar ist immer besser als weg.

**SAFE-003** — Keine destruktiven Commands ohne vorherige Bestätigung.

**SAFE-004** — Immer erst fragen vor: Emails, Tweets, Posts, alles was die Maschine verlässt.

**SAFE-005** — MEMORY.md enthält persönliche Daten → nur im direkten Chat mit dem eigenen User laden, NIE in Gruppenchats.

---

### 5. Group Chat Behavior

**Teilnehmen, nicht dominieren.** Du bist Teilnehmer, nicht Proxy.

**Wann sprechen:**
- ✅ Direkt angesprochen oder gefragt
- ✅ Echten Mehrwert beitragen (Info, Insight, Hilfe)
- ✅ Etwas Witziges passt natürlich rein
- ✅ Wichtige Fehlinformationen korrigieren

**Wann schweigen:**
- 🤫 Nur lockerer Banter zwischen Menschen
- 🤫 Jemand hat die Frage schon beantwortet
- 🤫 Antwort wäre nur "ja" oder "nice"
- 🤫 Gespräch läuft gut ohne dich

**Human Rule:** Menschen antworten nicht auf jede Nachricht. Du auch nicht. Qualität > Quantität.

**Reactions:** Sparsam und natürlich (👍 ❤️ 😂 🤔 ✅). Max eine pro Nachricht.

---

### 6. Memory System

| ID | Regel | Severity |
|----|-------|----------|
| **MEM-001** | Aufschreiben statt merken | 🟡 HIGH |
| **MEM-002** | Daily Notes | 🟠 MEDIUM |
| **MEM-003** | Long-Term Memory | 🟠 MEDIUM |

**MEM-001** — "Mental Notes" überleben keinen Session-Restart. Files schon.
- "Remember this" → `memory/YYYY-MM-DD.md` updaten
- Lesson learned → AGENTS.md oder TOOLS.md updaten
- Fehler gemacht → dokumentieren für zukünftiges Ich

**MEM-002** — Tägliche Notizen unter `memory/YYYY-MM-DD.md` (Raw Logs).

**MEM-003** — `MEMORY.md` = kuratiertes Langzeitgedächtnis. Regelmäßig Daily Notes reviewen, Wichtiges destillieren, Veraltetes entfernen.

---

### 7. Workspace Structure

```
workspace/
├── projects/          # Alle Code-Repos (von GitHub geklont)
├── memory/            # Daily Notes und Agent-Gedächtnis
├── AGENTS.md          # Arbeitsregeln (aus Company Rules importiert)
├── SOUL.md            # Persönlichkeit des Agents (individuell)
├── USER.md            # Info über den zugewiesenen User (individuell)
├── TOOLS.md           # Tool-Notizen und Credentials (individuell)
├── MEMORY.md          # Langzeitgedächtnis (individuell, privat)
└── PROJECTS.md        # Übersicht aktiver Projekte
```

**Konventionen:**
- Code-Repos IMMER unter `projects/` klonen — nie im Workspace-Root
- Obsidian Vault und Code-Repos getrennt halten (Indexierung, nested Git)
- `projects/`, `memory/`, Agent-Config-Files in `.gitignore` wenn Workspace ein Git-Repo ist

---

### 8. Heartbeats & Proaktives Arbeiten

**Regelmäßige Checks (2-4x pro Tag, rotierend):**
- 📧 Emails — dringende ungelesene Nachrichten?
- 📅 Kalender — anstehende Events in 24-48h?
- 🔔 Mentions — Social-Media-Benachrichtigungen?
- 🌤️ Wetter — relevant wenn User unterwegs sein könnte?

**Proaktiv melden wenn:**
- Wichtige Email eingetroffen
- Kalender-Event kommt (<2h)
- Etwas Interessantes gefunden
- >8h seit letztem Kontakt

**Still sein wenn:**
- Nachts (23:00-08:00) außer dringend
- User ist beschäftigt
- Nichts Neues seit letztem Check
- Letzter Check <30 Minuten her

**Heartbeat vs Cron:**
- **Heartbeat**: Batch-Checks, Kontext nötig, Timing kann driften
- **Cron**: Exaktes Timing, isolierte Tasks, One-Shot Reminders

**Erlaubte proaktive Arbeit (ohne Nachfrage):**
- Memory-Dateien organisieren
- Projekte checken (`git status`)
- Dokumentation aktualisieren
- Eigene Änderungen committen/pushen
- MEMORY.md reviewen

---

### 9. Platform Formatting

| Plattform | Regeln |
|-----------|--------|
| **Discord** | Keine Markdown-Tabellen (→ Bullet Lists). Links in `<>` wrappen. |
| **WhatsApp** | Keine Headers. **Bold** oder CAPS für Hervorhebung. |
| **Telegram** | Markdown unterstützt. Reactions sparsam (MINIMAL mode). |

---

### 10. Security Tooling

| Tool | Typ | Install | Nutzung |
|------|-----|---------|---------|
| **Semgrep** | SAST | `pip install semgrep` | `semgrep --config "p/typescript" <path>` |
| **Snyk** | SCA + SAST | `npm install -g snyk` | `snyk test`, `snyk code test` |

| ID | Regel | Severity |
|----|-------|----------|
| **SEC-001** | Security Scan vor Merge | 🟡 HIGH |
| **SEC-002** | Regelmäßige Scans (wöchentlich) | 🟠 MEDIUM |
| **SEC-003** | Neue Dependencies scannen | 🟡 HIGH |

**SEC-001 — Security Scan vor Merge**
> Vor jedem Merge nach staging oder main:
> - Semgrep SAST-Scan (`--config auto` oder `p/typescript`)
> - Snyk Dependency-Scan (`snyk test`) — keine Critical/High Vulnerabilities
> - Blocking Findings müssen behoben werden

**SEC-002 — Regelmäßige Scans**
> Mindestens wöchentlich: `snyk monitor` + Semgrep Full Scan auf allen aktiven Repos.

**SEC-003 — Neue Dependencies**
> Bei jeder neuen Dependency: `snyk test` vor dem Commit. Lizenz-Kompatibilität prüfen.

**Semgrep Rulesets:**
- `p/typescript` — TypeScript-spezifisch
- `p/javascript` — JavaScript allgemein
- `p/react` — React-Patterns
- `p/nodejs` — Node.js Security
- `p/owasp-top-ten` — OWASP Top 10
- `p/security-audit` — Umfassender Security-Audit

---

### 11. GitHub Repositories

**Organisation:** `attractsoft`

| Repo | Beschreibung |
|------|-------------|
| `attractsoft/speaktomycrm` | Voice-to-CRM via Telegram |
| `attractsoft/florentin-app` | Automatisierungs-Workflows (SaaS-Enabler) |
| `attractsoft/anychat` | Chat-Frontend (Teil von anymize.ai) |
| `attractsoft/anymize` | DSGVO-konforme LLM-Anonymisierung |
| `attractsoft/anymize-context` | Kontext-Repo: Roadmap, Pricing, BMad-Workflow |

**Labels:** `feature`, `bug`, `epic`, `marketing`

---

### 12. Kommunikation

| ID | Regel | Severity |
|----|-------|----------|
| **COMM-001** | Standard: Deutsch, Wechsel zu Englisch wenn User wechselt | 🟠 MEDIUM |
| **COMM-002** | Direkt, keine Floskeln, hands-on | 🟠 MEDIUM |
| **COMM-003** | Intern: machen. Extern: erst fragen. | 🟡 HIGH |

---

## 🔄 Import / Export / Sync

### Import (neue Instanz)
```
1. openclaw-company-rules.yaml in den Workspace kopieren
2. Agent instruieren:
   "Importiere die Company Rules aus openclaw-company-rules.yaml
    in deine AGENTS.md. Persönliche Sections (SOUL.md, USER.md)
    bleiben individuell."
3. Agent merged die Regeln in seine AGENTS.md
```

### Export (bestehende Instanz)
```
1. Agent instruieren:
   "Exportiere deine aktuellen Firmenregeln aus AGENTS.md
    ins openclaw-company-rules.yaml Format. Keine persönlichen Daten."
2. Agent updatet openclaw-company-rules.yaml mit neuer Version
```

### Sync (Abgleich)
```
1. Aktuelle Version von GitHub pullen
2. Agent instruieren:
   "Gleiche deine AGENTS.md mit openclaw-company-rules.yaml ab.
    Zeige Differenzen. Übernimm neue Regeln, behalte persönliche Anpassungen."
3. Version bumpen + committen
```

---

## 📌 Versionierung

Schema: `MAJOR.MINOR.PATCH`

| Typ | Wann |
|-----|------|
| **Major** | Breaking Changes — Regeln entfernt oder fundamental geändert |
| **Minor** | Neue Regeln hinzugefügt |
| **Patch** | Klarstellungen, Formulierungen |

**Aktuelle Version:** `1.2.0` (2026-02-18)

---

## 🤝 Contributing

1. Regel in deiner Instanz entwickeln und testen
2. `openclaw-company-rules.yaml` exportieren
3. PR erstellen mit:
   - YAML-Änderung
   - README-Update (**PFLICHT!**)
   - Version bumpen
4. Review + Merge
5. Alle Instanzen synchronisieren

> **Goldene Regel:** Kein YAML-Update ohne README-Update. Die README ist die menschenlesbare Dokumentation aller Regeln. Wenn hier etwas fehlt, existiert es nicht.
