---
name: divergent-frontend
description: Dauntless's frontend engineering playbook for Team Divergent. Use for any UI implementation, component architecture, state management, or client-side API integration work. Encodes component design rules, state handling, performance, and accessibility standards. Complements the Divergent Design System (DIVERGENT-DESIGN.md).
---

# Divergent Frontend — Dauntless's Playbook

Works WITH the Divergent Design System (`DIVERGENT-DESIGN.md`): that file governs visual/UX quality (the configurable tokens, surface rules, chart specs, and ten UX Laws); this playbook governs Dauntless's engineering craft. Emit its tokens into the styling layer, apply the resolved `config` (surfaceMode/theme/radius/accent), obey its ten UX Laws — then apply the craft below.

## Component rules
- Small, single-purpose components; if a component needs "and" to describe it, split it
- Props are the API: explicit, typed where the stack allows, no grab-bag `options` objects
- Presentational components never fetch; data flows down, events flow up
- Reuse before rebuild: before building ANY UI, search the codebase for an existing component FIRST, every time — reuse or extend it, never fork it

## Color system fidelity (Color Wheel Theory)
Amity owns the palette; you keep it intact in code. It's a harmony-based scheme (OKLCH/HSL, even tonal ramp) — you never dilute it.
- Colors come ONLY from Amity's named token palette. No raw/random hex, no one-off color in a component. A needed-but-missing color goes to `docs/specs/<feature-slug>/blockers.md` for Amity, OR gets added to the token file as a properly-derived, named token — never inlined.
- Implement semantic tokens (surface / text / border / action / success / warning / error) mapped onto the ramp. Components reference semantic tokens, never raw ramp values.
- Hold the 60-30-10 distribution in composition; ONE accent for the primary action — don't spread accent across the screen.
- Contrast is a build gate: verify WCAG AA (4.5:1 body text, 3:1 large text / UI). Never convey state by color alone — pair it with icon/text/shape.
- Forbidden: the generic purple→blue gradient and any other unmotivated gradient. A gradient must have a reason Amity specified.

## 8pt grid enforced by the setup
The grid is enforced by configuration, not willpower — wire it once so the scale is the only easy path.
- All layout-relevant spacing/sizing (margin, padding, gap, width/height) uses the 8px scale via tokens. 4px is allowed ONLY for fine typographic/icon nudges.
- No magic pixel values anywhere. Configure the styling system (Tailwind `theme.spacing`, CSS custom properties, or the design-token file) so off-scale values are the hard path, not the easy one.
- Line-heights preserve vertical rhythm. Touch/click targets ≥44px.

## One component system — single source of truth
Top priority for the owner: the WHOLE project renders from one component library and one token layer.
- One canonical primitive per role (one Button, one Input, one Card…) with typed variant props. Feature code composes primitives; it never re-styles from scratch.
- Before building anything, search for the existing component/token/pattern and reuse or extend it. Copy-paste-and-tweak is banned — add a variant to the canonical component instead.
- Every design value (color, space, type, radius, shadow, motion) resolves through the shared token layer. A component with a hardcoded value that bypasses tokens is a defect.
- When a pattern appears twice, extract it into the library. The component library + tokens ARE the project's single source of truth; keep them that way.

## State management discipline
- Server data, UI state, and form state are three different things — never mash them into one store
- Derive, don't duplicate: if a value can be computed from existing state, compute it
- Every async operation renders all four outcomes: idle, loading, success, error — no exceptions

## The API boundary
- ALL contract calls live in one thin client module; components never call HTTP directly
- Parse/validate responses at the boundary; the rest of the app trusts typed data
- Errors are handled where the user can act on them, with the copy Amity specified

## Performance defaults
- Ship less: no dependency for what 15 lines of code can do
- Images sized and lazy-loaded; lists virtualized past ~100 items; expensive work debounced
- Measure before optimizing anything else — no speculative micro-optimization

## Accessibility as engineering
Semantic HTML first (button is a `<button>`); keyboard path tested for every flow; focus managed on route/dialog changes; ARIA only where semantics can't do the job — wrong ARIA is worse than none.

## Quick-task recipes (the frontend slash commands)
These are the plugin's focused commands — each is a standard, repeatable pass over existing UI. Apply the recipe whether the user typed the command or asked in prose; the sections above are the law each recipe enforces.
- **`/responsive`** — make a page/component responsive, mobile-first (mobile → tablet → desktop). No overflow or horizontal body scroll; wide content scrolls in its own container; spacing on the 8pt grid; touch targets ≥44px. Verify each breakpoint, then lint + build.
- **`/fix-spacing`** — snap every margin/padding/gap/size to the 8pt scale via tokens; kill magic values (13/17/22px); equalize gaps between repeated items; fix vertical rhythm and proximity. Spacing only — don't touch color/type unless it blocks alignment.
- **`/fix-colors`** — replace raw/random hex with semantic palette tokens; hold Color Wheel harmony + 60-30-10 + one accent; add missing colors as derived named tokens (never inline); remove unmotivated gradients; verify WCAG AA contrast.
- **`/a11y`** — semantic HTML, full keyboard path + logical focus order, focus managed on route/dialog changes, ARIA only where semantics can't do the job, accessible names, reduced-motion, AA contrast. Verify with a keyboard-only walkthrough.
- **`/states`** — add every missing state (idle, loading, empty, error with retry + next step, success, disabled) with real microcopy, wired to the data layer. No happy-path-only screens.
- **`/polish`** — presentation-only pass: sharpen hierarchy (one primary action), fix alignment/typography, remove the AI-slop tells. Behavior and content unchanged; reuse the component system, never fork.
Every recipe finishes the same way: touch frontend files only, verify (build/lint, exercise the change), and report what changed.

## Anti-patterns (never do)
- `any`-typed escape hatches or ts-ignore to silence the compiler
- Copy-pasting a component and tweaking it instead of extracting a variant / forking the canonical primitive per feature
- Swallowing promise rejections; empty catch blocks
- A hardcoded color or spacing value that bypasses tokens — a raw/one-off hex, or an off-8pt-grid pixel — it's a defect, not a shortcut
- The templated "AI look" (anti-slop tells): Inter-for-everything by default, purple→blue or any unmotivated gradient, cards-in-cards, decorative icon tiles above headings, gray text on colored fills, gratuitous glassmorphism/shadows, emoji-as-icons
- Conveying a state by color alone; shipping without an idle/loading/empty/error/success/disabled state Amity specified
