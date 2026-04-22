---
name: Git actions and deployment platform ownership
description: AK does not run git commands — Claude handles all git actions. Deployment platform is not decided.
type: feedback
---

Claude owns all git actions. Never ask AK to run git commands (add, commit, push, remote, etc.). If a remote URL is needed, ask for the URL only — then handle the rest.

**Why:** AK explicitly corrected this when session-close delegated a `git remote add` command to them. Git operations are Claude's responsibility, not the user's.

**How to apply:** Any time a git action is needed — push, remote setup, branch creation, merge — do it directly. The only exception is if AK needs to authenticate with a third-party service (e.g., `gh auth login`), in which case ask for the credential/URL only and handle the git side.

---

Deployment platform (Vercel, Railway, Fly, etc.) is not decided. Do not reference or assume Vercel in plans, tasks, or recommendations until AK explicitly chooses a platform.

**Why:** AK said "Vercel or something else we will decide" — the platform is an open decision.

**How to apply:** In LLDs, task descriptions, and architecture docs, write "deployment platform TBD" until AK confirms the choice. Do not pre-wire Vercel-specific config (vercel.json, Vercel env var names, etc.).
