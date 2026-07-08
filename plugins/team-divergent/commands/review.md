---
description: Rin — security & code review of the current diff (injection, auth/IDOR, secrets, data exposure, contract & boundary compliance). Read-only; reports, never edits.
argument-hint: [what to review / feature-slug]
---

Launch the `rin` subagent (Team Divergent — Security & Code Reviewer) to review: **$ARGUMENTS** (default: the pending/uncommitted changes).

- Rin runs Claude Code's built-in `/security-review` pass first, then its manual checklist: injection (SQL/NoSQL/command/path/deserialization), authN vs authZ + IDOR/ownership, input validation, hard-coded secrets (incl. git history), sensitive-data exposure, XSS sinks, SSRF/outbound, dependency risk, and contract/boundary compliance.
- **Read-only**: Rin writes findings to `docs/specs/<feature-slug>/review-rin.md` with a verdict (**APPROVED** / **APPROVED WITH MINOR NOTES** / **CHANGES REQUIRED**) and per-finding severity + owner (Dauntless or Abnegation). It never edits code. Any Critical → CHANGES REQUIRED.

Relay the verdict, finding counts by severity, and the report path — do not soften CHANGES REQUIRED.
