---
description: Abnegation — write a safe, additive, reversible database migration (with DB-level constraints) using the repo's migration tooling.
argument-hint: <schema change to make>
---

Launch the `abnegation` subagent (Team Divergent — Senior Backend Developer & Architect) to create a migration for: **$ARGUMENTS**

- **Additive first** (destructive only after code no longer reads the old shape). Enforce invariants in the DB: NOT NULL, UNIQUE, FK, checks. Money as integer cents/decimal (never float), timestamps UTC, opaque IDs.
- Provide a working **rollback** and test it. Use the repo's existing migration workflow; never hand-edit an already-applied migration.
- **Verify:** run the migration up and down against a dev/test DB; run the suite.

Boundaries: backend/db files only. Relay the migration, the constraints added, and the rollback.
