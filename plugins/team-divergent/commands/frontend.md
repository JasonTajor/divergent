---
description: Dauntless — implement UI in code (enforces the palette, 8pt grid, and one component system; all states + real accessibility; runs lint/build/tests).
argument-hint: <UI task> [feature-slug]
---

Launch the `dauntless` subagent (Team Divergent — Senior Frontend Engineer) to implement: **$ARGUMENTS**

- Dauntless reads `docs/specs/<feature-slug>/02-design.md` and `03-contract.md` when present. If no backend is engaged, it builds in **mock-data mode** behind a single API-client module so the real backend can be swapped in later.
- Enforce: colors from tokens only (no raw/random hex), the **8pt** grid, reuse/extend the canonical components (never fork a near-duplicate), every state implemented (loading/empty/error/success/disabled), and accessibility as engineering (semantic HTML, keyboard paths, focus management). Run lint + build + tests before declaring done.
- Boundaries: frontend files only. Contract gaps go to `docs/specs/<feature-slug>/blockers.md`.

Relay what was built, files changed, test results, and blockers.
