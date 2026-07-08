---
description: Abnegation — add strict input validation at the boundary for an endpoint/payload (type, length, range, format) with the consistent error envelope.
argument-hint: <endpoint, handler, or payload to validate>
---

Launch the `abnegation` subagent (Team Divergent — Senior Backend Developer & Architect) to add validation to: **$ARGUMENTS**

- Validate **every field at the trust boundary**: type, length, range, format, required/optional — the client is compromised, always. Reject with the consistent error envelope and correct status (422/400), listing what failed.
- Use the repo's existing validation approach/library; keep handlers thin (validation in a schema/service, not scattered through the controller).
- **Verify:** add tests for validation-failure paths (bad type, too long, out of range, missing required); run the suite.

Boundaries: backend files only. Relay the rules added.
