---
description: Three — make a page/component fully responsive (mobile-first) across mobile/tablet/desktop on the 8pt grid, with no overflow or horizontal scroll.
argument-hint: [file, component, or page — defaults to recent UI changes]
---

Launch the `three` subagent (Team Divergent — Senior Frontend Engineer) to make responsive: **$ARGUMENTS** (if nothing is named, target the most recently changed UI files).

- **Mobile-first:** define behavior at mobile, then layer up tablet and desktop breakpoints. No fixed widths that overflow; content reflows, images `max-width:100%`, wide tables/code scroll inside their own `overflow-x:auto` container — the page body never scrolls sideways.
- Keep spacing on the **8pt grid** via tokens (no magic px). Touch targets ≥44px on small screens.
- Preserve the existing component system — adjust layout with the established responsive utilities/tokens; never fork a component.
- **Verify** each breakpoint and run lint + build before declaring done.

Boundaries: frontend files only. Relay files changed and how you verified each breakpoint.
