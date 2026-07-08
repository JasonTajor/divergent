---
description: Dauntless — a visual polish pass (hierarchy, alignment, typography, consistency) that removes the generic "AI-generated" look without changing behavior.
argument-hint: [file, component, or page — defaults to recent UI changes]
---

Launch the `dauntless` subagent (Team Divergent — Senior Frontend Engineer) to polish: **$ARGUMENTS** (default: recently changed UI files).

- Sharpen **hierarchy** (one clear primary action; secondary things visibly quieter), fix alignment and optical consistency, tighten **typography** (scale, weight, line-height) — all via tokens and the 8pt grid.
- Remove the **AI-slop tells**: Inter-for-everything with no reason, purple→blue gradients, cards-in-cards, decorative icon tiles above headings, gray text on colored fills, gratuitous glassmorphism/shadows, emoji-as-icons.
- **Presentation only** — behavior and content stay unchanged. Reuse the component system; don't fork.
- **Verify:** run lint + build.

Boundaries: frontend files only. Relay what you changed and why.
