---
name: divergent-backend
description: Four's backend engineering playbook for Team Divergent. Use for any API, database, authentication, or business-logic work. Encodes API design rules, data integrity, security defaults, and error-handling standards. Complements the Superpowers TDD skill.
---

# Divergent Backend — Four's Playbook

Works WITH Superpowers: TDD and systematic-debugging govern HOW Four works; this playbook governs WHAT good backend code looks like.

## Design before you build (think first)
Measure twice, cut once. The churn this team wants to kill — the endless fixing and refactoring — comes almost entirely from code that was written before it was designed. A short design rationale beats three rounds of rework. Before you write code, reason about and briefly WRITE DOWN (in the contract, a design note, or blockers.md if it exposes a gap):
- The **data model** and its access patterns (see below) — this is the decision everything else hangs off.
- The **read/write patterns and expected scale**: rough rows, RPS, and growth. Design for them now — indexes, pagination, and denormalization-only-where-justified — not after the first slow-query page.
- The **boundaries**: thin handlers, a testable service layer, clear module seams, so the next feature extends this one instead of rewriting it.
- **Idempotency, concurrency, and consistency**: decide the story up front — optimistic locking, unique constraints, transactional boundaries — so a race condition is designed out, not patched in after prod.
- **Design for change**: stable contracts, versioned APIs, additive migrations, feature-flag-friendly seams. Future work should be additive, not a refactor.
- A **performance budget per endpoint**: the target latency and query count you will build to.
Building the right thing once is cheaper than building the wrong thing three times. This design pass is not optional overhead — it IS the work that stops the refactor treadmill.

## Data model first
The data model is the hardest thing to change once there's data and code on top of it, and it's the root cause of most refactors. Get it right before anything else:
- **Model for how the data is READ, not just how it's stored.** Enumerate the queries the product needs to answer, then design the schema to answer them cheaply.
- Get relationships and normalization right first; denormalize only for a measured read pattern, with a written justification and an invalidation/consistency story.
- Every foreseeable access path has a supporting index (or a written reason it doesn't need one). Composite indexes match your actual WHERE/ORDER BY.
- Pick keys, types, and constraints deliberately — opaque IDs, UTC timestamps, money as integer cents or decimal (never float), NOT NULL/UNIQUE/FK enforced in the DB.
- A wrong schema shipped is a migration, a backfill, and a refactor later. Spend the twenty minutes now.

## API design rules
- The contract is law — but push back through blockers.md if the contract itself is flawed; never silently deviate
- One consistent error envelope everywhere; correct status codes (401 unauthenticated vs 403 unauthorized vs 404 not-found vs 422 validation — these are different things)
- Endpoints are nouns + verbs done by HTTP methods; no `/doThing` RPC-soup unless the contract says so
- Every list endpoint gets pagination from DAY ONE — retrofitting it is 10x the cost

## The trust boundary (memorize this)
The client is compromised. Always. Validate every field (type, length, range, format) at the boundary; authorize every request (authenticated ≠ allowed — check ownership/role for THIS resource); rate-limit anything expensive or abusable; never reflect raw input back into queries, shells, headers, or HTML.

## Data integrity
- Constraints live in the database (NOT NULL, UNIQUE, FK), not just in app code — the DB is the last line of defense
- Multi-step writes are transactional or they are bugs
- Migrations: additive first, destructive only after code no longer reads the old shape; always test the rollback
- Money is integers (cents) or decimals — never floats. Timestamps are UTC. IDs are opaque to clients.

## Error handling & logging
- Fail loudly internally, safely externally: log the stack trace, return the sanitized envelope
- Log auth failures, validation rejections, and slow queries with request IDs; never log passwords, tokens, or PII
- No empty catch blocks. No "TODO: handle this". Handle it or crash visibly.

## Performance as design (not an afterthought)
Performance is a design decision made up front, not a cleanup pass bolted on when it's already slow. Optimization is cheapest at the whiteboard and most expensive in a hotfix.
- **Every query hits an index or has a written justification.** Hunt N+1 in your OWN code before Rin ever sees it; SELECT only the columns you need — no lazy `SELECT *` on hot paths.
- **Set a performance budget per endpoint** (target latency, max query count) and design to it. If a design can't hit its budget, that's a design problem to solve now.
- **Pagination and limits on every list from day one**; bound every otherwise-unbounded operation. Retrofitting pagination is 10x the cost.
- **Cache only WITH an invalidation story** — understand cache/DB consistency. A cache without an invalidation plan is a data-corruption feature.
- **Prefer set-based / bulk operations over row-by-row loops**; push work into the database when the database does it better.
- **Know the cost of what you call.** Move expensive work to async/background jobs; use connection pooling; never block the request path on slow I/O you could defer.
- Avoid premature micro-optimization — but NEVER ship an obviously O(n²), N+1, or full-table-scan design. That avoidable churn is exactly what this team is done paying for.

## Optimization / anti-churn mindset
Build it once, correctly. The right abstraction and correct behavior up front are cheaper than the fix-refactor treadmill.
- **DRY without over-abstracting.** Two call sites is a coincidence; three is a pattern. Missing abstraction causes copy-paste bugs; premature abstraction causes rewrites — both are churn.
- **Enforce invariants in the DB**, not just in app code — the database is the last line of defense and the thing that survives a buggy deploy.
- **Exhaustive error handling**: no empty catch, no "TODO: handle later." Every failure path is handled or crashes loudly.
- **Tests lock in behavior** so refactors are safe and cheap — behavior stays covered when the internals change.

## Observability & operability (catch it before it's an emergency)
Problems you can see early never become emergency refactors.
- **Structured logging with request IDs** across the request lifecycle — correlate a failure end to end.
- **Metrics / timing on hot paths**; log slow queries with their parameters (never the secret ones).
- Expose meaningful **health** of the system (dependencies reachable, not just "process up").
- Never log secrets, tokens, or PII — observability is not an excuse to leak.

## Quick-task recipes (the backend slash commands)
These are the plugin's focused commands — each is a standard, repeatable pass over existing server code. Apply the recipe whether the user typed the command or asked in prose; the sections above are the law each recipe enforces.
- **`/optimize`** — hunt N+1 (per-row → set-based/eager fetch), add indexes matching real WHERE/ORDER BY, stop `SELECT *` on hot paths, bound unbounded queries, prefer bulk ops. Behavior unchanged; measure before→after; run the suite.
- **`/secure-endpoint`** — authenticated ≠ allowed: add per-resource ownership/role checks (fix IDOR), rate-limit abuse, parameterized queries, sanitized I/O, consistent error envelope + correct status codes. Add auth-failure / unauthorized-access tests.
- **`/validate`** — validate every field at the boundary (type, length, range, format, required/optional); reject with the consistent envelope (422/400) listing failures; keep it in a schema/service, not scattered through the handler. Add validation-failure tests.
- **`/paginate`** — add pagination (cursor or limit/offset per repo convention) with a default + max page size, bounded at the DB; consistent items+page-metadata shape; index-backed ORDER BY; keep the contract in sync. Test first/last/over-max page.
- **`/migration`** — additive-first, reversible migration with DB-level constraints (NOT NULL/UNIQUE/FK/checks), correct types (integer cents/decimal money, UTC, opaque IDs); working, tested rollback; use the repo's migration workflow. Run up and down.
Every recipe finishes the same way: touch backend files only, run the test suite, and report what changed (with before→after cost for `/optimize`).

## Anti-patterns (never do)
- Coding before the data model and access patterns are decided
- String-built SQL under any circumstance
- Business logic in controllers/handlers — thin handlers, testable services
- Returning different response shapes for the same endpoint depending on code path
- Unbounded / unpaginated queries and unbounded loops
- N+1 by design — fanning out per-row queries instead of a set-based fetch
- Full-table-scan or obviously O(n²) designs shipped as "we'll optimize later"
- Premature abstraction AND missing abstraction — both breed churn
- Caching without an invalidation story
- Denormalizing without a measured read pattern and a consistency plan
- Empty catch blocks or "TODO: handle later"
