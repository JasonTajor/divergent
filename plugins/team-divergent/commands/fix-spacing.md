---
description: Dauntless — audit and fix spacing/layout rhythm to the 8pt grid (margins, padding, gaps), removing magic values and inconsistent gaps.
argument-hint: [file, component, or page — defaults to recent UI changes]
---

Launch the `dauntless` subagent (Team Divergent — Senior Frontend Engineer) to fix spacing on: **$ARGUMENTS** (default: recently changed UI files).

- Snap every margin/padding/gap/dimension to the **8pt scale** (4px only for fine type/icon nudges) via spacing tokens — replace hardcoded/odd values (13px, 17px, 22px…).
- Enforce consistent vertical rhythm and proximity: related items sit closer than unrelated ones; equalize gaps between repeated items.
- Spacing only — leave colors and typography as-is unless they block alignment. Reuse the token scale; don't invent one-offs.
- **Verify:** run lint + build; confirm layout is unchanged except tightened spacing.

Boundaries: frontend files only. Relay the before→after values you changed.
