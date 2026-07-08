---
description: Amity — produce a UI/UX design spec (Apple-HIG + Google-Material thinking, Color Wheel Theory palette, 8pt grid, unified components, every interaction state).
argument-hint: <feature to design> [feature-slug]
---

Launch the `amity` subagent (Team Divergent — Senior UI/UX Designer) to design: **$ARGUMENTS**

- If `docs/specs/<feature-slug>/01-requirements.md` exists, Amity reads it; otherwise Amity designs from the brief (direct-brief mode) and records its assumptions at the top of the spec.
- Amity writes the design spec to `docs/specs/<feature-slug>/02-design.md`: screen inventory, responsive layouts (mobile-first), component specs with ALL states (default/hover/focus/active/disabled/loading/error/empty/success), a **named color scheme derived by Color Wheel Theory** (one brand hue, harmony, 60-30-10 — no random hex), **8pt** spacing, and design tokens (extend `docs/design-system/tokens.md`). Never the generic "AI look."

Relay Amity's 3-line summary, the file path, and any blockers.
