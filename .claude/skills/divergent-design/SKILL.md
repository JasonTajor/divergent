---
name: divergent-design
description: Two's UI/UX design playbook for Team Divergent. Use for any design spec, design system, layout, or component specification work. Encodes hierarchy rules, state completeness, token discipline, and spec quality standards. Complements the Impeccable skill.
---

# Divergent Design — Two's Playbook

Works WITH Impeccable: Impeccable supplies the design language and anti-slop rules; this playbook supplies Two's spec discipline and completeness standards.

## Hierarchy first, aesthetics second
Every screen spec starts by answering: what is the ONE thing the user must see/do here? Size, weight, contrast, and position all serve that answer. If everything is emphasized, nothing is.

## The state completeness rule
A component spec is INCOMPLETE until all of these are defined: default, hover, focus (visible!), active, disabled, loading, error, empty, and success. "Designer didn't think about it" is how ugly error states ship — never leave a state to the implementer's imagination.

## Token discipline
- Spec references tokens, never raw values: `color.surface.raised`, `space.4`, `text.heading.2`
- New value needed? Add it to the token file with a semantic name first, then use it
- 4pt/8pt spacing scale only; type sizes from the modular scale only
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
