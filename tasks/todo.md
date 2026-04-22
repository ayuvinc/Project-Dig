# tasks/todo.md

## SESSION STATE
Status:         OPEN
Active task:    none
Active persona: none
Blocking issue: none
Last updated:   2026-04-22

---

## Active Tasks


### TASK-002 — Scaffold Next.js application
## Objective
Initialize the Next.js project (App Router, TypeScript, Tailwind CSS) so `npm run dev`, `npm run build`, `npm run lint`, and `npm run test` all have something to run against.
## Scope
- `npx create-next-app@latest` with: App Router, TypeScript, Tailwind, ESLint, src/ directory
- Add `jest` + `@testing-library/react` for test suite (npm run test)
- Confirm `tsconfig.json` has strict mode enabled
- Confirm `next.config.ts` exists
## Acceptance Criteria
- `npm run dev` starts dev server without errors
- `npm run build` completes without errors
- `npm run lint` runs without configuration errors
- `npm run test` runs (even with 0 tests — should not error)
- No `any` types in generated scaffold
## Security Notes
- No secrets in scaffold code
- Clerk middleware added in TASK-011+ — not scope here
## Dependencies
- TASK-001
## Status
PENDING

---

### TASK-003 — Resolve audit log path conflict
## Objective
Two audit log files and two path references point at different locations. Reconcile to a single canonical path: `releases/audit-log.md` (as specified in CLAUDE.md override).
## Scope
- Update `settings.json` MCP server env: `"AUDIT_LOG_PATH": "releases/audit-log.md"`
- Migrate content of `tasks/audit-log.md` into `releases/audit-log.md`
- Verify column order in `releases/audit-log.md` matches MCP server parser format: `| Timestamp | Agent | Status | Run ID | Summary |`
- Delete `tasks/audit-log.md` after migration
## Acceptance Criteria
- Only one audit log file: `releases/audit-log.md`
- `settings.json` MCP `AUDIT_LOG_PATH` = `releases/audit-log.md`
- All prior entries preserved
- Column order matches MCP `get_recent_entries()` positional parser
## Security Notes
- Audit log is a compliance artifact — no entries deleted, only migrated
## Dependencies
- None
## Status
PENDING

---

### TASK-004 — Fix schemas/ root path mismatch
## Objective
Agent commands and hooks reference `schemas/audit-log-schema.md` and `schemas/output-envelope.md` at root level. No `schemas/` directory exists at root; actual files are at `framework/schemas/`. This generates warnings every session.
## Scope
Create root-level `schemas/` symlink pointing to `framework/schemas/` so all existing references resolve without touching every agent command file.
Affected references:
- `validate-envelope.sh`: `See schemas/output-envelope.md`
- `audit-log` skill: `schemas/audit-log-schema.md`
## Acceptance Criteria
- `ls schemas/audit-log-schema.md` resolves without error
- `ls schemas/output-envelope.md` resolves without error
- No "file not found" warning from audit-log or validate-envelope.sh during normal session run
## Security Notes
- N/A
## Dependencies
- None
## Status
PENDING

---

### TASK-005 — Document all active hooks in CLAUDE.md
## Objective
`settings.json` has 7 hooks active that are NOT in the CLAUDE.md hooks table, creating invisible behavior.
## Scope
Add to CLAUDE.md Hooks table:
PostToolUse on Write|Edit (currently undocumented):
  - `set-teach-me-flag.sh` — flags files for /teach-me auto-trigger
  - `set-codex-prep-flag.sh` — flags files for /codex-prep auto-trigger
  - `set-codex-read-flag.sh` — flags files for /codex-read auto-trigger
UserPromptSubmit (currently undocumented):
  - `guard-boundary-flags.sh` — blocks prompts when open BOUNDARY_FLAGs exist
  - `auto-teach.sh` — auto-triggers /teach-me on flagged files
  - `auto-codex-prep.sh` — auto-triggers /codex-prep on flagged files
  - `auto-codex-read.sh` — auto-triggers /codex-read on flagged files
## Acceptance Criteria
- CLAUDE.md Hooks table documents all 12 active hooks
- Each entry: hook name, trigger event, enforcement purpose
## Security Notes
- N/A
## Dependencies
- None
## Status
PENDING

---

### TASK-006 — BA discovery: populate planning docs
## Objective
`docs/problem-definition.md`, `docs/scope-brief.md`, `docs/hld.md` contain only placeholder text. No BA discovery has occurred. The guard passes (files exist) but content is not real. No design can be validated.
## Scope
- Run /ba discovery conversation with AK
- Populate `docs/problem-definition.md`: primary user, pain, workaround, failure mode, success outcome
- Populate `docs/scope-brief.md`: Session 1 must-haves, should-haves, cut-for-now, delivery target
- Update `docs/hld.md` to draft with real system overview
- Update status frontmatter from `draft` → `confirmed` after AK approves
## Acceptance Criteria
- No placeholder text in problem-definition.md or scope-brief.md
- Status = confirmed on both docs
- AK has explicitly approved content
## Security Notes
- N/A
## Dependencies
- None — this is prerequisite to all design work
## Status
PENDING

---

### TASK-007 — Populate ba-logic.md for Session 1
## Objective
`tasks/ba-logic.md` is empty. Architect is BLOCKED with MISSING_BA_SIGNOFF on any design task until BA has written confirmed business logic decisions.
## Scope
- BA writes Session 1 decisions: auth model, who can submit, search constraints, rating rules
- At minimum: auth model (which routes are protected, what unauthenticated users can see)
## Acceptance Criteria
- `tasks/ba-logic.md` has at least one confirmed decision per Session 1 feature area
- Each decision has `confirmed_by: BA`, `date: 2026-04-22`
## Security Notes
- Auth model decisions here feed into the security checklist for every derived task
## Dependencies
- TASK-006 (scope-brief confirmed before BA can write logic decisions)
## Status
PENDING

---

### TASK-008 — Populate memory/MEMORY.md project identity
## Objective
`memory/MEMORY.md` Project Identity section has all template placeholder values. Read at every session open by all personas — currently provides no value.
## Scope
Fill in:
- Name: Project-Dig
- Stack: Next.js (App Router), TypeScript, Neon (PostgreSQL + pgvector), Clerk, OpenAI Embeddings (text-embedding-3-small), Tailwind CSS, Vercel
- Repo: [AK provides GitHub URL after TASK-001]
- Framework version: 3.0.0
## Acceptance Criteria
- No `[PLACEHOLDER]` text in Project Identity section
- Stack accurate and complete
## Security Notes
- N/A
## Dependencies
- TASK-001 (repo URL available once git initialized and remote added)
## Status
PENDING

---

### TASK-009 — Fix guard-persona-boundaries.sh: read Active persona from todo.md
## Objective
`guard-persona-boundaries.sh` reads `ACTIVE_PERSONA` env var, which is never set. The guard has zero enforcement. Fix: read `Active persona` field directly from `tasks/todo.md` at hook invocation time.
## Scope
- Modify `guard-persona-boundaries.sh` to parse `Active persona:` from `tasks/todo.md` when `ACTIVE_PERSONA` env var is not set
- Test: with `Active persona: junior-dev`, confirm writes to `tasks/todo.md` are blocked
- Test: with `Active persona: none`, confirm guard is inactive (no persona = no enforcement, framework-level work)
## Acceptance Criteria
- Junior Dev persona correctly blocked from writing `tasks/todo.md`, `types/`, `lib/`, `middleware/`, `releases/`
- QA persona correctly blocked from writing source code
- BA persona correctly restricted to `tasks/ba-logic.md`, `docs/`, `channel.md`
- Guard passes for Architect (no restrictions)
## Security Notes
- This is an active security enforcement boundary — must be reliable and tested
## Dependencies
- None
## Status
PENDING

---

### TASK-010 — Create LLD for Session 1 Foundation
## Objective
`docs/lld/` has no feature LLD files. CLAUDE.md requires LLD before Junior Dev can implement. Session 1 Foundation (auth, types, CI/CD) has no design document.
## Scope
Create `docs/lld/session-1-foundation.md`:
- Auth setup: Clerk middleware, protected /dashboard routes, public routes
- Type definitions: Submission, Embedding, Insight, Rating (in types/)
- Database schema: Neon PostgreSQL tables with pgvector column
- CI/CD: Vercel deployment, GitHub Actions lint+test gate
- Folder structure and naming conventions
## Acceptance Criteria
- LLD covers all Session 1 scope items from scope-brief.md
- Security checklist completed for each section
- Junior Dev can implement without needing to ask design questions
## Security Notes
- Auth model: unauthenticated users blocked on /dashboard, Clerk session validation enforced
- No client-side DB access — all queries via server actions
- OpenAI key never exposed to client
## Dependencies
- TASK-006 (BA scope confirmed)
- TASK-007 (BA logic decisions available)
## Status
PENDING

---

## Backlog

### TASK-011 — Run /risk-manager
## Objective
`tasks/risk-register.md` is empty. Run /risk-manager after AK approves task plan above to populate risk register for Session 1.
## Status
PENDING

---

### TASK-012 — Configure .env.local
## Objective
No environment variables configured. Create `.env.local` with Neon DB URL, Clerk keys, OpenAI API key. Verify `.gitignore` protects this file.
## Status
PENDING

---

### TASK-013 — Populate tasks/design-system.md
## Objective
Design system is full placeholder. Junior Dev needs this before any UI work. Blocked on /designer or /ux completing visual direction work.
## Status
PENDING (blocked on UX/Designer running first)

---

## Archive

### TASK-001 — Initialize git repository — DONE 2026-04-22
- `git init` on main, 121 files committed
- `.gitignore` updated: added `node_modules/`, `.next/`, `.DS_Store`, `tasks/.session-transition-lock`
- First commit: `902310d`
- Note: git user.name/user.email used hostname defaults — AK should run `git config --global user.name` / `git config --global user.email`
