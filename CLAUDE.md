# CLAUDE.md — Project-Dig
# Crowdsourced knowledge platform — aggregates community input and uses AI embeddings to surface structured, searchable insights (not Reddit/Twitter noise)
# Stack: Next.js, TypeScript, Neon (PostgreSQL + pgvector), Clerk, OpenAI Embeddings, Tailwind CSS, Vercel
Tier: Standard  # MVP | Standard | High-Risk — controls which gates are active (see framework/governance/operating-tiers.md)

---

## First Thing on Session Start

Ask: "I've read the CLAUDE.md. What role am I playing — Architect, Junior Developer, Business Analyst, UI/UX Designer, or QA?"

Read your **Role Card** (`.claude/commands/<role>.md`) and state it aloud before touching any task.

---

## Commands

```bash
# Replace with your project's actual commands
npm run dev        # Local dev server
npm run build      # Production build (catches TS/compile errors)
npm run lint       # Linter
npm run test       # Test suite
```

---

## The Team

**AK — Product Manager (human, the boss).** Owns requirements, priorities, final approvals.

---

## Planning Artifacts (docs/)

| Doc | Status | Required Before |
|-----|--------|-----------------|
| docs/problem-definition.md | [draft/confirmed] | Any build work |
| docs/scope-brief.md | [draft/confirmed] | Any build work |
| docs/hld.md | [draft/confirmed] | Multi-feature sprint |
| docs/lld/<feature>.md | [draft/confirmed] | Feature implementation |
| docs/assumptions.md | [maintained] | Ongoing |
| docs/decision-log.md | [maintained] | Ongoing |
| docs/release-truth.md | [draft/confirmed] | Demo or release |
| docs/traceability-matrix.md | [maintained] | Task derivation |

All planning docs are conversation-derived. See guides/11-conversation-first-planning.md.

---

## Workflow

```
Discovery → Problem/Scope docs → HLD → LLD → Tasks → Build → QA → Release

AK → gives requirements
BA      → discovery conversation → confirms problem-definition.md + scope-brief.md
UI/UX   → user flow + wireframe + interaction rules (ux-specs.md)
Architect → reads BA + UX outputs → drafts hld.md → AK approval
Architect → creates lld/<feature>.md for each feature → derives tasks to todo.md (PENDING)
Architect → creates feature branches
QA      → adds acceptance criteria to each PENDING task
Junior Dev → IN_PROGRESS → builds to spec → READY_FOR_QA
CI      → lint + build + tests (must pass)
Architect → code review → approve or return to Junior Dev
UI/UX   → reviews built UI against wireframe
QA      → QA_APPROVED or QA_REJECTED
Architect → archive → merge to main → present to AK → write next-action.md
```

End of session: `tasks/todo.md` and `tasks/ba-logic.md` empty.

---

## SESSION STATE

Session state lives in `tasks/todo.md`. All personas read it there. `/session-open` transitions CLOSED→OPEN. `/session-close` transitions OPEN→CLOSED. See `schemas/state-machine.md` for the full contract.

---

## Hooks (Automated Enforcement)

The following hooks are configured in `.claude/settings.json` and run automatically by the Claude Code harness. These are MECHANICAL checks — Claude does not need to manually re-check what hooks already enforce.

**PreToolCall hooks** (run before Write/Edit/Bash):

| Hook | Enforces |
|---|---|
| `guard-session-state.sh` | Blocks unauthorized writes to SESSION STATE in tasks/todo.md |
| `guard-persona-boundaries.sh` | Enforces persona CAN/CANNOT file-path restrictions |
| `guard-git-push.sh` | Blocks git push to main unless Architect with QA_APPROVED |
| `guard-planning-artifacts.sh` | Blocks Junior Dev from writing source code when planning docs missing (Standard + High-Risk tiers only; MVP exempt) |

**PostToolCall hooks** (run after Write/Edit):

| Hook | Enforces |
|---|---|
| `set-teach-me-flag.sh` | Flags modified files for /teach-me auto-trigger |
| `set-codex-prep-flag.sh` | Flags modified files for /codex-prep auto-trigger |
| `set-codex-read-flag.sh` | Flags modified files for /codex-read auto-trigger |

**PostToolCall hooks** (run after Bash):

| Hook | Enforces |
|---|---|
| `auto-audit-log.sh` | Auto-appends audit entries after skill execution |
| `validate-envelope.sh` | Warns if skill output is missing required envelope fields |

**UserPromptSubmit hooks** (run on each user prompt):

| Hook | Enforces |
|---|---|
| `auto-persona-detect.sh` | Reads next-action.md and hints which persona to activate |
| `guard-boundary-flags.sh` | Blocks prompts when open BOUNDARY_FLAGs exist |
| `auto-teach.sh` | Auto-triggers /teach-me on flagged files |
| `auto-codex-prep.sh` | Auto-triggers /codex-prep on flagged files |
| `auto-codex-read.sh` | Auto-triggers /codex-read on flagged files |

**Stop hooks** (run when session ends):

| Hook | Enforces |
|---|---|
| `session-integrity-check.sh` | Warns if session is still OPEN when exiting |

---

## Plan Mode — Mandatory Triggers

| Condition | Rule |
|---|---|
| Task touches more than 2 files | Mandatory |
| Task modifies types | Mandatory |
| New data model or schema change | Mandatory |
| Modifies shared services | Mandatory |
| No BA sign-off on business logic | Mandatory |
| Hotfix with uncertain scope | Mandatory |
| No confirmed problem-definition.md or scope-brief.md | Mandatory |
| Feature work without corresponding LLD | Mandatory |
| Release packaging without release-truth.md | Mandatory |

---

## Definition of Done

- [ ] Code reviewed by Architect — no `any`, correct patterns, no security issues
- [ ] Build passing, tests passing
- [ ] Access control verified — unauthenticated users cannot access protected data
- [ ] UI reviewed by UI/UX against wireframe
- [ ] Mobile layout checked at 375px
- [ ] Security model verified — auth enforced, data boundaries correct, no secrets in code
- [ ] Observability verified — errors logged, key actions traceable, no silent failures
- [ ] Edge cases tested: empty state, error state, unauthenticated access
- [ ] Task logged to `releases/session-N.md` before deletion
- [ ] Task traces to scope item + HLD section + LLD file (traceability-matrix.md)
- [ ] No open BOUNDARY_FLAG entries

---

## Escalation Path

| Situation | Who handles it |
|---|---|
| Code bug | QA → Junior Dev (QA_REJECTED) |
| Built UI doesn't match wireframe | UI/UX → Junior Dev (REVISION_NEEDED) |
| Architectural flaw | QA → Architect (written escalation) |
| Business logic question | Architect → BA (ba-logic.md) |
| UX question | Architect → UI/UX (ux-specs.md) |
| Scope/priority change | Any persona → AK |

---

## Git Branching

- Branch name: `feature/TASK-XXX-short-description`
- Junior Dev creates branch before writing any code
- Commits to branch only, never to `main`
- Architect merges after QA_APPROVED; deletes branch after merge

---

## Project Overview

Project-Dig is a crowdsourced knowledge platform for knowledge workers, researchers, and analysts. Users submit knowledge and the platform uses AI embeddings to convert raw community input into structured, searchable insights — without the noise of Reddit or Twitter.

Core user journeys:
1. **Submit** — User posts a knowledge contribution (article, insight, link, or raw text)
2. **Discover** — User searches structured insights surfaced by AI embeddings from the community pool
3. **Rate** — User rates insight quality, feeding back into the curation model

---

## Tech Stack

- **Framework:** Next.js (App Router)
- **Language:** TypeScript
- **Database:** Neon (serverless PostgreSQL + pgvector for embeddings)
- **Auth:** Clerk
- **AI SDK:** OpenAI Embeddings (text-embedding-3-small)
- **CSS:** Tailwind CSS
- **Hosting:** Vercel

---

## Architecture Rules

- Server components by default; `'use client'` only when interactivity required
- All DB access via server actions or API routes — never from client components
- Clerk middleware protects all `/dashboard` routes — unauthenticated users redirect to sign-in
- No `any` types — strict TypeScript enforced
- Embeddings generated server-side only — OpenAI key never exposed to client

---

## Environment Variables

```
# Replace with your project's actual env vars
# Never commit .env.local
```

---

## Domain Types

- **Submission** — a unit of crowdsourced knowledge (text, link, or file) submitted by a user
- **Embedding** — vector representation of a Submission stored in pgvector
- **Insight** — AI-structured output derived from clustered Embeddings
- **Rating** — user feedback on an Insight (useful/not useful)

All types live in `types/` and are derived from the Neon schema.

---

## Session Roadmap

| Session | Focus | Status |
|---|---|---|
| 1 | Foundation — auth, types, CI/CD | Pending |
| 2 | [Next feature] | Pending |

---

## Session Start Checklist

- [ ] Read SESSION STATE in tasks/todo.md — run /session-open to transition CLOSED→OPEN
- [ ] Read Role Card (`.claude/commands/<role>.md`), state it aloud
- [ ] Read `tasks/next-action.md` — confirm expected persona
- [ ] Run standup (done / next / blockers)
- [ ] Read last 10 entries of `tasks/lessons.md`
- [ ] Resolve open BOUNDARY_FLAGs (Architect only)

---

## Audit Log

Path override for this project (set to your audit log file path):

```
audit_log: releases/audit-log.md
```

Agents resolve `releases/audit-log.md` from this CLAUDE.md field at runtime.

---

## Intake Summary

- **Risk level:** Medium
- **Compliance:** none
- **AI features:** Yes
- **Primary user:** Knowledge workers, researchers, analysts
- **Success metric:** 100 active contributors in 30 days, 70% useful rating on AI insights
- **Generated by:** AK-Cognitive-OS bootstrap v3.0.0
