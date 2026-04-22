# tasks/todo.md

## SESSION STATE
Status:         CLOSED
Active task:    none
Active persona: none
Blocking issue: none
Last updated:   2026-04-22T00:00:00Z — bootstrapped by bootstrap-project.sh v3.1.0
---

## Active Tasks

<!-- empty — all tasks archived or deferred to Session 2 -->



## Backlog
<!-- All items below are deferred to Session 2 -->

### TASK-010 — Create LLD for Session 1 Foundation
## Objective
Create `docs/lld/session-1-foundation.md`: Clerk auth, TypeScript domain types, Neon schema, CI/CD pipeline. Required before Junior Dev can implement.
## Dependencies
- BA-001 through BA-006 confirmed ✓
## Status
PENDING — Session 2 first task

---

### TASK-009 — Fix guard-persona-boundaries.sh: read Active persona from todo.md
## Objective
Guard reads `ACTIVE_PERSONA` env var which is never set — zero enforcement. Fix to read from `tasks/todo.md` directly.
## Security Notes
- Active security enforcement boundary
## Status
PENDING — Session 2

---

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
