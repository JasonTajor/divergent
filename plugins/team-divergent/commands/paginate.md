---
description: Four — add pagination + limits to a list endpoint (and bound any unbounded query), with a consistent page/response shape.
argument-hint: <list endpoint or query to paginate>
---

Launch the `four` subagent (Team Divergent — Senior Backend Developer & Architect) to add pagination to: **$ARGUMENTS**

- Add pagination (cursor or limit/offset — match the repo's convention) with a sane **default** and a **max page size**; bound the query at the database, not in memory.
- Return a consistent shape (items + page metadata / next cursor); keep it aligned with the API contract and update `docs/specs/<feature-slug>/03-contract.md` if one exists.
- Ensure the ORDER BY + pagination is **index-backed**.
- **Verify:** add tests for first page, last page, and over-max page size; run the suite.

Boundaries: backend files only. Relay the shape and limits chosen.
