# tasks/todo.md

## SESSION STATE
Status:         OPEN
Active task:    none
Active persona: none
Blocking issue: none
Last updated:   2026-04-22

---

## Active Tasks






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

### TASK-008 — Populate memory/MEMORY.md project identity — DONE 2026-04-22
- Name, stack, framework version filled in; repo URL pending GitHub remote

### TASK-005 — Document all active hooks in CLAUDE.md — DONE 2026-04-22
- 7 previously undocumented hooks added to CLAUDE.md Hooks table

### TASK-004 — Fix schemas/ root path mismatch — DONE 2026-04-22
- symlink created: schemas/ → framework/schemas/
- schemas/audit-log-schema.md and schemas/output-envelope.md both resolve

### TASK-003 — Resolve audit log path conflict — DONE 2026-04-22
- settings.json MCP AUDIT_LOG_PATH updated to releases/audit-log.md
- tasks/audit-log.md deleted (historical entries preserved in releases/audit-log.md)

### TASK-007 — Populate ba-logic.md — DONE 2026-04-22
- BA-001 through BA-006 confirmed by AK via discovery interview
- Commit: `7c0cfba`

### TASK-006 — BA discovery: populate planning docs — DONE 2026-04-22
- docs/problem-definition.md: confirmed, all sections populated
- docs/scope-brief.md: confirmed, Session 1 must-haves defined
- docs/hld.md: still template — deferred to Session 2 (HLD requires LLD first)
- Commit: `7c0cfba`

### TASK-002 — Scaffold Next.js application — DONE 2026-04-22
- Next.js 16.2.4, App Router, TypeScript (strict), Tailwind v4, ESLint
- Jest 30 + @testing-library/react + jest-dom configured
- `npm run build` ✓ | `npm run lint` ✓ | `npm run test --passWithNoTests` ✓
- Commit: `5fe7789`

### TASK-001 — Initialize git repository — DONE 2026-04-22
- `git init` on main, 121 files committed
- `.gitignore` updated: added `node_modules/`, `.next/`, `.DS_Store`, `tasks/.session-transition-lock`
- First commit: `902310d`
- Note: git user.name/user.email used hostname defaults — AK should run `git config --global user.name` / `git config --global user.email`
