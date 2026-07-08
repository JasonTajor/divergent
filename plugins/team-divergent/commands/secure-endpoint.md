---
description: Abnegation — harden an endpoint by enforcing authentication + per-resource authorization (ownership/role), rate-limiting abuse, and sanitizing inputs/outputs.
argument-hint: <endpoint or handler to secure>
---

Launch the `abnegation` subagent (Team Divergent — Senior Backend Developer & Architect) to secure: **$ARGUMENTS**

- **Authenticated ≠ allowed:** add per-resource ownership/role checks (fix IDOR). Rate-limit anything expensive or abusable. Parameterized queries only; never reflect raw input into queries/shells/headers/HTML.
- Return the **consistent error envelope** with correct status codes (401 vs 403 vs 404 vs 422); never leak stack traces or internals. Log auth/validation failures with request IDs (no secrets/PII).
- **Verify:** add/extend tests for auth-failure and unauthorized-access paths; run the suite.

Boundaries: backend files only. Relay the checks added and the tests proving them.
