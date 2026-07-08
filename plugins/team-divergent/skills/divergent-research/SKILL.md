---
name: divergent-research
description: Erudite's UX research and requirements playbook for Team Divergent. Use for any requirements gathering, user research, flow mapping, or acceptance criteria work. Encodes questioning techniques, edge-case taxonomies, and spec quality standards.
---

# Divergent Research — Erudite's Playbook

## The Five Whys of every feature request
Before accepting a feature as stated, answer: (1) Who exactly is this for? (2) What are they doing today without it? (3) What breaks or hurts in that current path? (4) How will we KNOW this fixed it? (5) What's the cost of building the wrong version? If any answer is "unknown", it becomes an open question in the spec — never a silent assumption.

## Research method (evidence, then design)
1. **Observe what exists** — grep the repo for real signal before theorizing: analytics/telemetry hooks, error logs, prior specs, support/ticket notes, TODOs about user pain. Behavior beats speculation.
2. **Map the full user spectrum** — enumerate primary, secondary, edge, and marginalized/accessibility users (see the inclusive checklist). One persona is a red flag, not a finish line.
3. **Frame the job** — for each key segment, write the job-to-be-done: "When [situation], I want to [motivation], so I can [outcome]." The feature is a candidate solution, not the goal.
4. **Classify every claim** — tag it SAY / DO / ASSUME (below). Assumptions become open questions or testable success criteria.
5. **Define how you'll know it worked** — measurable usability signals so design is validated against research, not vibes.

## Say vs Do vs Assume (the evidence standard)
- **DO** = observed behavior (analytics, session recordings, prior usage, support logs). Strongest evidence — weight it highest.
- **SAY** = stated preference (interviews, surveys, requests). Real but unreliable: users mispredict their own behavior. Corroborate against DO where possible.
- **ASSUME** = a hypothesis with no evidence yet — including your own and the stakeholder's. Legitimate only when labeled `[ASSUMPTION]` and paired with what would confirm/refute it.
The cardinal sin is laundering an ASSUME into a DO. When behavioral evidence is missing, write "no behavioral evidence — assumption" rather than asserting.

## Inclusive-by-default checklist (walk EVERY feature)
"For every user" is a mandate, not aspiration. For each, ask: can this user complete the job, and what do they experience?
- **Screen-reader / non-visual**: is the flow operable and announced without sight? (labels, order, focus, live regions)
- **Low-vision**: zoom to 200%, high-contrast, no meaning conveyed by color alone
- **Motor**: keyboard-only, switch, voice — no hover-only or precise-drag-only actions; adequate target size
- **Cognitive**: plain language, forgiving flows, no time traps, recoverable errors
- **Low-bandwidth / older device**: does it work on a slow connection and a weak CPU?
- **Non-native-language / i18n**: translatable strings, RTL, name/date/number/currency formats
- **First-time vs power user**: onboarding empty state AND efficient repeat use
WCAG AA is the floor, never the ceiling. An un-checkable segment is an explicit open question, never a silent omission.

## Requirement quality bar
A requirement is well-formed only if it is: **testable** (a QA engineer could pass/fail it), **atomic** (one behavior per FR), **implementation-free** (no component names, endpoints, or libraries), and **unambiguous** (no "fast", "easy", "user-friendly" without a measurable definition).

## Edge-case taxonomy (walk ALL of these, every time)
- **Empty**: zero items, first-time user, no data yet
- **Extreme**: maximum length input, thousands of items, huge files
- **Invalid**: wrong type, malformed, injection-shaped input
- **Interrupted**: network drop mid-action, double-click/double-submit, back button
- **Unauthorized**: logged out mid-session, wrong role, expired token, direct URL access
- **Concurrent**: two users/tabs editing the same thing
- **Temporal**: timezones, expired content, clock skew, "what if this runs at midnight Dec 31"
- **Human**: accessibility (screen reader, keyboard-only, low vision), localization, slow devices

## Acceptance criteria format
Given/When/Then, one criterion per behavior, each mapped to an FR number. Include at least one NEGATIVE criterion per flow (Given invalid X, Then the system must NOT...).

## Anti-patterns (never do)
- Solution-shaped requirements ("add a dropdown") — capture the need, not the widget
- Averaging conflicting stakeholder wishes into mush — surface the conflict explicitly
- Padding the spec with obvious filler to look thorough — every line must inform a decision
- **Designing for yourself** — your fluency, device, language, and eyesight are not the user's; verify, don't project
- **Single-persona tunnel vision** — one happy primary persona hides the secondary, edge, and marginalized users where features actually break
- **Happy-path-only** — a spec with no failure/error/empty/offline journeys is half a spec
- **Opinion dressed as evidence** — presenting an ASSUME or a SAY as settled fact; if it isn't DO, label it
