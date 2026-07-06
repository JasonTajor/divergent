---
name: divergent-backend
description: Four's backend engineering playbook for Team Divergent. Use for any API, database, authentication, or business-logic work. Encodes API design rules, data integrity, security defaults, and error-handling standards. Complements the Superpowers TDD skill.
---

# Divergent Backend — Four's Playbook

Works WITH Superpowers: TDD and systematic-debugging govern HOW Four works; this playbook governs WHAT good backend code looks like.

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

## Performance defaults
- Every query hits an index or has a written justification
- N+1 queries are hunted in review of your own code before Rin ever sees it
- Cache only with an invalidation story; a cache without one is a data-corruption feature

## Anti-patterns (never do)
- String-built SQL under any circumstance
- Business logic in controllers/handlers — thin handlers, testable services
- Returning different response shapes for the same endpoint depending on code path
