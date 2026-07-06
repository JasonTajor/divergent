---
name: divergent-design
description: Two's UI/UX design playbook for Team Divergent. Use for any design spec, design system, layout, or component specification work. Encodes hierarchy rules, state completeness, token discipline, and spec quality standards. Complements the Impeccable skill.
---

# Divergent Design — Two's Playbook

Works WITH Impeccable: Impeccable supplies the design language and anti-slop rules; this playbook supplies Two's spec discipline and completeness standards.

The bar: design like Apple's HIG team AND Google's Material research team at once. Apple gives you clarity, deference (content over chrome), and depth. Google gives you systematic, evidence-driven decisions for the full spectrum of users. Both insist on the same thing — intent over decoration, and every choice earned.

## Research-driven decisions (no taste-by-vibes)
Design is a chain of justified decisions, not a mood board. Before any layout, state:
- **WHO** — the primary user (from One's spec). Not "users"; a specific person with a specific job.
- **WHAT** — their goal and context: what they're trying to accomplish, on what device, under what pressure (rushed, distracted, first-time, expert).
- **WHY** — for every key layout decision, the reason it serves that goal. Each cites a One FR number or a stated user-behavior rationale ("primary action bottom-right because thumb reach on mobile", not "looks balanced").
Apply the Apple lens: reduce cognitive load, defer to platform conventions, ONE primary action per screen, let content be the interface, favor direct manipulation. Apply the Google lens: design for accessibility, internationalization, and device/context diversity from the start; treat usability as measurable. If a decision can't be traced to a user need, it's decoration — cut it or justify it in writing.

## Hierarchy first, aesthetics second
Every screen spec starts by answering: what is the ONE thing the user must see/do here? Size, weight, contrast, and position all serve that answer. If everything is emphasized, nothing is. One primary action per screen; everything else is visibly secondary.

## The state completeness rule
A component spec is INCOMPLETE until all of these are defined: default, hover, focus (visible!), active, disabled, loading, error, empty, and success. "Designer didn't think about it" is how ugly error states ship — never leave a state to the implementer's imagination.

## Color system (Color Wheel Theory)
No random colors, ever. A palette is derived, not picked.
- **Name the scheme.** Every spec declares which color-wheel scheme it uses and why: monochromatic, analogous, complementary, split-complementary, or triadic. The choice follows the product's emotional job (calm/utility → mono or analogous; energy/contrast → complementary or triadic).
- **One brand hue, then derive.** Choose a single primary/brand hue. Every other color comes from harmonic relationships to it (its complement, its ±30° neighbors, its triad partners). You never open a picker and grab a pretty hex.
- **60-30-10 distribution.** 60% dominant/neutral surface, 30% secondary, 10% accent/action. The 10% is where the eye goes — spend it on the primary action, not on chrome.
- **Hue-tinted neutrals.** Neutrals are tinted toward the brand hue (low chroma), never pure gray-127. Cohesion comes from every surface sharing a whisper of the brand.
- **Perceptual space + even ramps.** Work in OKLCH (or HSL) so lightness/chroma steps are visually even. Build a consistent tonal ramp per hue (e.g. 50–900) with predictable steps; light and dark modes map to the same ramp.
- **Semantic roles derived in-harmony.** success/warning/error/info are tuned to sit within the palette's harmony (shift their hue/chroma toward the scheme), not bolted-on stock red/green/yellow.
- **Contrast is a hard gate.** WCAG AA minimum — 4.5:1 body, 3:1 large text/UI — verified with measured ratios, never eyeballed. Never use color as the only signal (pair with icon/text/weight).
- **Forbidden:** arbitrary/random hex values; unmotivated gradients — especially the generic purple→blue "AI" gradient; more than one accent competing for attention.

## 8pt spatial system
Space is a system, not a nudge-fest.
- Every margin, padding, gap, and component dimension snaps to the 8px baseline: 4, 8, 16, 24, 32, 40, 48, 64… (4px is allowed ONLY for tight typographic/icon adjustments — optical alignment, icon insets).
- Type follows a modular scale (single ratio, e.g. 1.2/1.25/1.333); line-heights are set to preserve vertical rhythm against the 8pt baseline.
- Touch targets ≥44px. Related things sit closer than unrelated things — proximity encodes grouping.
- No arbitrary spacing. A one-off must be justified in writing in the spec, or it doesn't exist.

## Component unification & single source of truth
The product must feel like one thing, made by one mind.
- **Inventory before you invent.** Before designing anything new, survey existing tokens, components, and patterns in the repo. Reuse or extend them. Never fork a near-duplicate.
- **One canonical component per role.** One Button system with variants (primary/secondary/ghost/destructive), not five buttons. Variants over new components, always.
- **Single source of truth.** All values reference named tokens; maintain/extend `docs/design-system/tokens.md` so every future feature inherits the same colors, spacing, and type. New value needed? Add it there with a semantic name first, then reference it.
- **Consistent interaction & voice.** The same control behaves the same everywhere; spacing rhythm and microcopy voice hold across every screen. A user should never have to relearn your product mid-flow.

## Token discipline
- Spec references tokens, never raw values: `color.surface.raised`, `space.4`, `text.heading.2`
- New value needed? Add it to the token file with a semantic name first, then use it
- 8pt spacing scale (4px only for tight type/icon adjustments); type sizes from the modular scale only
- Any one-off value must be justified in writing in the spec, or it doesn't exist

## Accessibility non-negotiables
Contrast AA minimum (AAA for body text where feasible), visible focus indicators on every interactive element, touch targets ≥44px, never color as the only signal, defined focus order per screen, motion respects reduced-motion preference.

## Microcopy rules
Real words in specs, never lorem ipsum. Buttons say what they do ("Save changes", not "OK"). Error messages say what happened AND what to do next. Empty states teach, they don't apologize.

## Spec quality bar
Three (frontend) must be able to build the screen WITHOUT asking a single question. If a reasonable engineer could interpret a spec line two ways, rewrite it.

## Anti-patterns (never do)
- Designing the happy path only
- Restyling existing components when a variant would do — extend the system, don't fork it
- Decoration that doesn't serve hierarchy, comprehension, or brand
- Making decisions by vibes — any choice you can't trace to a user need is decoration

### The generic "AI-generated" tells — explicitly forbidden
These are the fingerprints of templated, thoughtless design. If your spec has any of them, it's wrong:
- Defaulting to Inter (or one font for everything) with no reason — typeface is a decision, make it
- Purple→blue (or any) gradient used as brand shorthand instead of a real palette
- Cards nested inside cards inside cards — flatten the hierarchy
- Decorative icon tiles/badges floating above headings for no informational purpose
- Evenly-gray body text laid over colored panels (fails contrast, reads as filler)
- Glassmorphism / blur / meaningless drop shadows applied for "polish" rather than depth cues
- Emoji used as iconography in a product UI
- Everything center-aligned and evenly spaced with no focal point
Restraint and hierarchy over decoration. The design must have a point of view.
