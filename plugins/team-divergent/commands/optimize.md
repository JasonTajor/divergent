---
description: Four — find and fix backend performance problems — N+1 queries, missing indexes, SELECT *, unbounded queries, row-by-row loops.
argument-hint: [endpoint, query, module, or file — defaults to recent backend changes]
---

Launch the `four` subagent (Team Divergent — Senior Backend Developer & Architect) to optimize: **$ARGUMENTS** (default: recently changed backend code).

- Hunt **N+1** (per-row queries → set-based/eager fetch), add missing **indexes** that match the actual WHERE/ORDER BY, stop `SELECT *` on hot paths, **bound** unbounded queries, and prefer bulk/set-based ops over row-by-row loops.
- Confirm every query hits an index or has a written justification; set/verify a per-endpoint **performance budget** (target latency, max query count).
- **Behavior must not change** — this is optimization only. Measure before→after where the tooling allows (query count / timing).
- **Verify:** run the test suite; confirm results are identical.

Boundaries: backend files only. Relay each optimization with its before→after cost.
