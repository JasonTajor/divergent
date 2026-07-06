---
name: divergent-frontend
description: Three's frontend engineering playbook for Team Divergent. Use for any UI implementation, component architecture, state management, or client-side API integration work. Encodes component design rules, state handling, performance, and accessibility standards. Complements the Impeccable skill.
---

# Divergent Frontend — Three's Playbook

Works WITH Impeccable: Impeccable governs visual execution quality; this playbook governs Three's engineering craft.

## Component rules
- Small, single-purpose components; if a component needs "and" to describe it, split it
- Props are the API: explicit, typed where the stack allows, no grab-bag `options` objects
- Presentational components never fetch; data flows down, events flow up
- Reuse before rebuild: search the codebase for an existing component FIRST, every time

## State management discipline
- Server data, UI state, and form state are three different things — never mash them into one store
- Derive, don't duplicate: if a value can be computed from existing state, compute it
- Every async operation renders all four outcomes: idle, loading, success, error — no exceptions

## The API boundary
- ALL contract calls live in one thin client module; components never call HTTP directly
- Parse/validate responses at the boundary; the rest of the app trusts typed data
- Errors are handled where the user can act on them, with the copy Two specified

## Performance defaults
- Ship less: no dependency for what 15 lines of code can do
- Images sized and lazy-loaded; lists virtualized past ~100 items; expensive work debounced
- Measure before optimizing anything else — no speculative micro-optimization

## Accessibility as engineering
Semantic HTML first (button is a `<button>`); keyboard path tested for every flow; focus managed on route/dialog changes; ARIA only where semantics can't do the job — wrong ARIA is worse than none.

## Anti-patterns (never do)
- `any`-typed escape hatches or ts-ignore to silence the compiler
- Copy-pasting a component and tweaking it instead of extracting a variant
- Swallowing promise rejections; empty catch blocks
- Styling that bypasses the token system with magic values
