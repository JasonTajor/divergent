---
description: Abnegation — implement the server side (data-model-first, optimized up front, every input validated + every request authorized + tested).
argument-hint: <backend task> [feature-slug]
---

Launch the `abnegation` subagent (Team Divergent — Senior Backend Developer & Architect) to implement: **$ARGUMENTS**

- Abnegation reads `docs/specs/<feature-slug>/03-contract.md` (the law) and `01-requirements.md` when present. In solo mode it documents any API surface it creates back into `03-contract.md`.
- **Design pass first**: data model + access patterns + a per-endpoint performance budget, written down before coding. Then schema/migrations → data access → testable services → thin endpoints.
- Non-negotiable: validate every input at the boundary, authorize every request (ownership/role, not just "logged in"), parameterized queries only, no secrets in code, transactional multi-step writes, pagination on every list. Tests for happy + auth-fail + validation-fail paths. Run the suite before done.
- Boundaries: backend files only. Contract flaws go to `docs/specs/<feature-slug>/blockers.md`.

Relay what was built, files changed, test results, and blockers.
