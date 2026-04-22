---
Status: confirmed
Source: user-confirmed
Last confirmed with user: 2026-04-22
Owner: Product
Open questions: 2
---

# Scope Brief

## Must-Have (Session 1 — Foundation)

- [x] Clerk authentication — sign-in, sign-up, session management
- [x] Protected /dashboard routes — unauthenticated users redirected to sign-in
- [x] TypeScript domain types — Submission, Embedding, Insight, Rating
- [x] Neon PostgreSQL schema — tables matching domain types, pgvector extension enabled
- [x] CI/CD pipeline — lint + TypeScript + tests gate on every push to main

## Must-Have (Session 2 — Submit + Discover MVP)

- [ ] Submit a link or text post with optional tags (authenticated)
- [ ] Public search — semantic search returns AI insights with source submissions beneath
- [ ] Tag browsing — public, no auth required
- [ ] Rate an insight useful / not useful (authenticated, idempotent)

## Should-Have (Session 3+)

- [ ] AI insight generation — cluster submissions by topic, generate structured insight summaries
- [ ] Embedding pipeline — generate and store vectors for all submissions
- [ ] Rating-weighted search ranking
- [ ] Empty state experiences (no results, no insights yet for a tag)
- [ ] Search autocomplete

## Cut-for-Now

- [ ] User profiles beyond Clerk identity
- [ ] Submission editing or deletion by user
- [ ] URL content fetching/indexing (link posts store URL only)
- [ ] File uploads or image attachments
- [ ] Tag moderation, merging, or aliasing
- [ ] Comments or threaded discussion
- [ ] Following users or topics
- [ ] Notifications

## Explicit Out-of-Scope

- Social graph (followers, following, DMs)
- Web crawling or external content indexing
- Anonymous submissions
- Downvoting or report/flag system (no moderation tools in MVP)
- Mobile app (web only)

## Delivery Target

| Field | Value |
|-------|-------|
| Target | pilot / early access |
| Success metric | 100 active contributors in 30 days, 70% useful rating on AI insights |
| Deadline | TBD — AK to confirm |

## Constraints and Dependencies

| Constraint | Impact |
|-----------|--------|
| OpenAI Embeddings (text-embedding-3-small) | Embedding cost scales with submission volume — 10,000 char cap on text submissions |
| Neon serverless PostgreSQL | Cold start latency on first request — acceptable for MVP, monitor in pilot |
| Clerk for auth | No custom auth logic — Clerk owns the auth surface entirely |
| Vercel deployment | Serverless functions only — no long-running background jobs at MVP |

## Open Questions

1. Minimum submission threshold before an insight is generated (suggest: 3+ submissions on a topic) — Architect to define
2. Can a submitter rate an insight derived from their own submission? (BA suggests: yes, allowed) — AK to confirm
