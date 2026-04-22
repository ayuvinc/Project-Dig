# channel.md — Agent Communication Channel

## Purpose

This file is the shared communication bus between agents (personas and skills).
Each agent reads context here and writes its output status here.
The Architect and skill agents own the write gate.

---

## Current Status

```
session:        none
sprint:         none
active_persona: none
last_skill_run: session-close
last_updated:   2026-04-22T00:00:00Z
```

---

## Last Handoff

```yaml
from:    session-close
to:      Architect
status:  PASS
message: "Session 3 closed (test only — MCP server test, no tasks completed). Next: Session 4, Architect writes docs/lld/session-1-foundation.md (TASK-010)."
```

---

## Queued Messages

<!-- Agents append messages here. Architect clears at session close. -->

---

## Sprint Packet Status

```
packet_ready:     false
codex_ready:      false
last_intake_run:  —
```

---

## Usage

- Skills write their output envelope summary here after execution
- Architect reads here to understand current sprint state
- `/check-channel` reads this file and reports status to the team
- `/session-close` clears stale entries and archives the session state
