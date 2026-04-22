# tasks/ba-logic.md

## Purpose

BA writes confirmed business logic decisions here.
Architect reads before designing. Junior Dev reads before building.
Cleared at end of sprint after tasks are archived.

---

## Active Decisions

---

### BA-001 — Primary User & Problem
- Status: CONFIRMED
- Scope: Who this product is for and what pain it solves
- confirmed_by: AK
- date: 2026-04-22

**User outcome:** Any knowledge worker (corporate researcher, independent analyst, academic, or generalist) can come to Project-Dig to find structured, community-sourced insights on any topic — whether for deep research or solving an immediate personal problem.

**Business rules:**
  - The platform serves all knowledge worker archetypes — no single persona is primary
  - The core value proposition is "Reddit but with insights" — community contributes raw knowledge, the platform surfaces structured signal
  - Use cases span professional research AND personal problem solving
  - The product is explicitly NOT a social network — community is the input mechanism, not the end product

**Edge cases:**
  - A user with no specific query must still be able to browse and discover value
  - A user solving a personal problem (non-professional) must feel equally served as a researcher

**Out of scope:** Social features (following users, DMs, profiles beyond identity), content moderation tools (Session 1+)

**Open questions for AK:** None — confirmed.

---

### BA-002 — Submission
- Status: CONFIRMED
- Scope: What users can submit and who can do it
- confirmed_by: AK
- date: 2026-04-22

**User outcome:** An authenticated user can post a knowledge contribution — either a URL link or free-form text — with optional tags, in under 60 seconds.

**Business rules:**
  - Submission types: link (URL + title + optional body) OR text (title + body). Both supported.
  - Submitter must be authenticated — no anonymous submissions
  - Each submission must have: a title, a type (link | text), and an author (user ID)
  - Tags are optional, free-form strings — no fixed taxonomy
  - A submission may have zero or more tags
  - A link submission must include a valid URL; body text is optional
  - A text submission must include body content; no URL required

**Edge cases:**
  - Empty title → blocked, validation error shown
  - Link submission with invalid/unreachable URL → accept and store as-is (no URL validation at MVP; fetching URL content is out of scope)
  - Text submission with empty body → blocked, validation error shown
  - Unauthenticated user attempts to submit → redirect to sign-in, return to submit form after auth
  - Duplicate submission (same URL already exists) → warn user, allow submission anyway (community may add different context)
  - Extremely long text body → cap at 10,000 characters (embedding cost control); show character count

**Out of scope:** URL content fetching/indexing, file uploads, rich text formatting, submission editing after posting, submission deletion by user (Session 2+), image attachments

**Open questions for AK:** None — confirmed.

---

### BA-003 — Discovery / Search
- Status: CONFIRMED
- Scope: How users find knowledge on the platform
- confirmed_by: AK
- date: 2026-04-22

**User outcome:** Any visitor (authenticated or not) can search Project-Dig and see AI-derived insights as primary results, with the source submissions visible underneath each insight for verification.

**Business rules:**
  - Search is publicly accessible — no login required to search or read
  - Search returns AI-derived insights as the primary result unit, not raw submissions
  - Each insight result links back to the source submissions that generated it
  - Semantic search powered by OpenAI embeddings (text-embedding-3-small)
  - Search must work with natural language queries, not just keyword matching
  - Insights with higher useful ratings must rank higher in results (rating signal feeds ranking)

**Edge cases:**
  - Empty search query → show trending or recently active insights (no blank state)
  - No results found → show "No insights yet on this topic" with a prompt to submit
  - Search with single character or junk input → return no results gracefully, no error
  - Unauthenticated user clicks "Submit" from search results → redirect to sign-in

**Out of scope:** Filtering by tag in search (Session 2+), search history, saved searches, search autocomplete (Session 2+)

**Open questions for AK:** What happens when there are not enough submissions to generate a meaningful insight cluster? (Minimum submission threshold for insight generation — Architect to define, suggest 3+ submissions on a topic.)

---

### BA-004 — Rating
- Status: CONFIRMED
- Scope: How users signal insight quality
- confirmed_by: AK
- date: 2026-04-22

**User outcome:** An authenticated user can rate any insight as "useful" or "not useful." They can change their rating at any time. Their latest rating replaces their previous one.

**Business rules:**
  - Rating values: useful | not_useful (binary, no neutral)
  - Only authenticated users can rate — no anonymous ratings
  - One rating per user per insight — idempotent upsert (last vote wins)
  - A user may change their rating at any time; the previous rating is replaced, not accumulated
  - Insight's aggregate useful percentage = (useful votes / total votes) × 100
  - The 70% useful threshold is the platform success metric — not a hard block on display
  - Ratings feed back into search ranking (higher useful % = higher rank)
  - A user may not rate their own insight (to be defined: does the submitter of the source submissions own the insight? Architect to clarify insight ownership model)

**Edge cases:**
  - User rates, then immediately re-rates (double-click) → idempotent, no duplicate entry
  - User rates an insight with zero prior votes → creates first rating entry
  - Unauthenticated user attempts to rate → prompt to sign in, return to insight after auth
  - Insight deleted (future) → cascade delete ratings

**Out of scope:** Rating individual submissions (only insights are rated), rating comments (comments not in scope), public display of who rated what (anonymous aggregates only)

**Open questions for AK:** Can a user who submitted a source submission rate the insight derived from their submission? Suggest: yes, allowed — the insight is a derivative work, not their submission directly.

---

### BA-005 — Tags & Topic Organisation
- Status: CONFIRMED
- Scope: How submissions are categorised
- confirmed_by: AK
- date: 2026-04-22

**User outcome:** A submitter can add free-form tags to their submission. Visitors can browse submissions and insights by tag without a search query.

**Business rules:**
  - Tags are optional, free-form strings added at submission time
  - No fixed taxonomy — tags are community-defined
  - A submission may have 0 to 5 tags (cap prevents tag spam)
  - Tags are normalised to lowercase, trimmed of whitespace
  - Tags must be 2–30 characters each
  - Tag browsing is publicly accessible (no login required)

**Edge cases:**
  - Submitter enters a tag with leading/trailing spaces → normalise silently
  - Submitter enters a tag that is all numbers or special characters → allow (no restriction at MVP)
  - Submitter enters duplicate tags on the same submission → deduplicate silently
  - No submissions exist for a tag → show empty state with prompt to submit
  - Tag with > 30 characters → truncate or block with validation message (Architect decides)

**Out of scope:** Tag moderation, tag merging/aliasing (e.g. "AI" and "ai" as one), tag following/subscriptions, trending tags (Session 2+)

**Open questions for AK:** None — confirmed.

---

### BA-006 — Session 1 Scope (Foundation)
- Status: CONFIRMED
- Scope: What ships in Session 1 and what does not
- confirmed_by: AK
- date: 2026-04-22

**User outcome:** The engineering foundation is in place — auth works, domain types are defined in TypeScript and database, CI/CD is configured. No user-visible features exist yet.

**Business rules:**
  - Session 1 deliverables: Clerk auth integration, TypeScript domain types, Neon database schema, CI/CD pipeline
  - Session 1 does NOT ship any user-facing feature (submit form, search UI, rating UI)
  - The /dashboard route must be protected by Clerk middleware — unauthenticated users redirect to sign-in
  - Public routes (/, /sign-in, /sign-up) must be accessible without auth
  - Domain types must match the confirmed data model from BA-001 through BA-005:
      Submission: id, type (link|text), title, body?, url?, authorId, tags[], createdAt
      Embedding: id, submissionId, vector (pgvector), model, createdAt
      Insight: id, summary, sourceSubmissionIds[], usefulCount, notUsefulCount, createdAt
      Rating: id, userId, insightId, value (useful|not_useful), createdAt, updatedAt
  - CI/CD: lint + TypeScript check + tests must pass before any merge to main

**Edge cases:**
  - Unauthenticated request to /dashboard → 302 redirect to /sign-in
  - Clerk webhook misconfigured → auth fails gracefully with a 500 error page, not a crash
  - Database connection failure → server action returns a typed error, not an unhandled exception

**Out of scope:** Any UI beyond Next.js default scaffold and Clerk sign-in/sign-up pages, AI embedding generation (Session 2+), insight generation (Session 2+), search UI (Session 2+)

**Open questions for AK:** None — confirmed.
