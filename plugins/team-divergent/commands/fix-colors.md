---
description: Dauntless — replace raw/random colors with harmony-based palette tokens (Color Wheel Theory), fix contrast failures, and remove unmotivated gradients.
argument-hint: [file, component, or page — defaults to recent UI changes]
---

Launch the `dauntless` subagent (Team Divergent — Senior Frontend Engineer) to fix colors on: **$ARGUMENTS** (default: recently changed UI files).

- Replace every raw/random hex or one-off color with the correct **semantic palette token** (surface/text/border/action/success/warning/error). If a needed color is missing, add it to the token file as a properly derived, named token — never inline a new hex.
- Enforce **Color Wheel** harmony and the 60-30-10 distribution; one accent for the primary action. Remove the generic purple→blue and other unmotivated gradients.
- **Contrast is a gate:** WCAG AA (4.5:1 body, 3:1 large/UI); never use color as the only state signal (pair with icon/text/weight).
- **Verify:** run lint + build.

Boundaries: frontend files only. Relay the tokens applied and any contrast fixes.
