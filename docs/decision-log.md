---
Status: draft
Source: mixed
Last confirmed with user: YYYY-MM-DD
Owner: Product
Open questions: 0
---

# Decision Log

## Decisions Made

| # | Date | Decision | Made by | Reasoning | Alternatives Considered | Downstream Effect |
|---|------|----------|---------|-----------|------------------------|-------------------|
| | | | | | | |

## Pending Decisions

| # | Decision Needed | Blocking | Options | Owner | Deadline |
|---|----------------|----------|---------|-------|----------|
| 1 | Should Project-Dig include a dedicated AI submission layer alongside human submissions? | HLD for Session 3+ | See INSIGHT-001 below | AK | Before Session 3 planning |

---

## Product Insights (Unconfirmed — Awaiting AK Decision)

### INSIGHT-001 — AI Submission Layer

**Date captured:** 2026-04-22
**Raised by:** Architect / product exploration
**Status:** UNCONFIRMED — needs AK decision before any design work

**The idea:**
Alongside human submissions, allow AI agents to continuously submit structured knowledge into the platform. An AI agent is pointed at a domain (e.g. "oncology trial failures", "startup fundraising 2025") and ingests, summarises, and submits knowledge into the pool on an ongoing basis — without waiting for human contributors.

**Why it matters:**

1. **Solves cold-start.** The platform currently has no answer to the empty-state problem — no submissions means no insights, no insights means no reason to join. AI submissions can seed the knowledge base before the first human arrives.

2. **Creates a two-tier knowledge pool.** Human submissions bring lived experience, opinions, and things the internet hasn't fully indexed. AI submissions bring breadth, structure, and coverage of topics humans don't bother writing about. Embeddings run over both tiers together — the resulting insights are richer than either alone.

3. **Repositions the product category.** Today the gap is narrow: between Reddit (noisy community) and Perplexity (AI web search). Adding an AI submission layer shifts Project-Dig from "crowdsourced knowledge aggregator" to "continuously updated, AI-built, human-validated knowledge intelligence layer" — a meaningfully larger and more defensible category. No product today combines AI-generated breadth + human submission intent + community rating validation.

**Risks to design around:**

- **Transparency.** If AI submissions are unlabelled, users who expect community knowledge feel deceived. Submission source (human | ai) must be a first-class field — and users should be able to filter or weight by source.
- **Quality floor.** AI submissions are fluent but can be wrong. Without a review gate or a strong flag mechanism, bad AI knowledge enters the embedding pool. A flag/review layer is currently out of scope — this needs to be designed before AI submissions go live.
- **"Crowdsourced" claim weakens.** The rating system becomes the trust mechanism: if practitioners mark AI-sourced insights as useful, the content earns its place regardless of origin. Design the rating system to surface source transparency.

**Proposed scope position:** Should-Have for Session 4+ (after human submit/discover/rate is working and proven). Do not build AI submissions until the human rating loop is validated — the rating signal is what gives AI content its quality gate.
