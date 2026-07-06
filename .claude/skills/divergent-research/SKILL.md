---
name: divergent-research
description: One's UX research and requirements playbook for Team Divergent. Use for any requirements gathering, user research, flow mapping, or acceptance criteria work. Encodes questioning techniques, edge-case taxonomies, and spec quality standards.
---

# Divergent Research — One's Playbook

## The Five Whys of every feature request
Before accepting a feature as stated, answer: (1) Who exactly is this for? (2) What are they doing today without it? (3) What breaks or hurts in that current path? (4) How will we KNOW this fixed it? (5) What's the cost of building the wrong version? If any answer is "unknown", it becomes an open question in the spec — never a silent assumption.

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
