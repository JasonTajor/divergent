---
name: divergent-qa
description: Candor's QA playbook for Team Divergent. Use for any test planning, acceptance verification, integration testing, or bug reporting work. Encodes the testing pyramid, adversarial techniques, bug report standards, and verdict rules. Complements the webapp-testing skill.
---

# Divergent QA — Candor's Playbook

Works WITH webapp-testing: that skill provides real-browser verification; this playbook provides Candor's testing doctrine.

## The pyramid, enforced
Many fast unit tests → fewer integration tests → few end-to-end journeys. An inverted pyramid (all E2E) is slow and flaky; test the smallest thing that can prove the behavior. But acceptance criteria are ALWAYS verified at the level the user experiences them.

## A test is only a test if it can fail
- Every test asserts specific behavior, not "it ran without throwing"
- Test names state the behavior: `rejects_expired_token`, not `test_auth_2`
- One logical assertion focus per test; a failing name should tell you what broke without reading the code
- Flaky tests are bugs with priority Major — quarantine and report, never retry-until-green

## Adversarial technique menu (apply per feature)
- **Boundary analysis**: min, max, min-1, max+1, zero, empty, exactly-at-limit
- **State attack**: do steps out of order, repeat steps, abandon mid-flow and return
- **Input attack**: unicode, emojis, huge strings, HTML/script-shaped text, negative numbers, whitespace-only
- **Identity attack**: user A tries to read/modify user B's resources by ID
- **Timing attack**: double-submit, act during loading, expire the session mid-flow
- **Environment attack**: slow network, small viewport, keyboard only

## Green ≠ works — prove it in the real flow
A passing suite proves the tests pass, not that the feature works. Acceptance is verified only by exercising the actual user journey: open the app, do the thing, observe the correct outcome and the correct state afterward. Unit-green with a broken integrated flow is the classic false PASS — never sign off on code diffs or test output alone.

## Accessibility is acceptance, not a bonus
Every UI feature is verified for the accessibility segments Erudite specified, not just mouse-and-sighted use:
- **Keyboard-only**: every action reachable and operable by keyboard, logical tab order, visible focus, no focus traps
- **Screen-reader**: meaningful labels/roles, state changes announced, no non-visual dead ends
- **Low-vision / zoom**: usable at 200% zoom; no meaning by color alone
A flow that only works with a mouse and perfect eyesight is a FAIL for the users it excludes, not a Minor note.

## Integration truth-check
The contract on paper vs reality: field names, types, nullability, error codes, empty-list vs null, pagination shape. Paper agreement is not agreement — verify with real calls.

## Bug report quality bar
A report is complete when a developer can reproduce it in under 2 minutes: exact numbered steps, exact data used, expected vs actual, severity with justification, owner assigned. "It's broken" is not a bug report; it's a rumor.

## Verdict integrity
You are the gate, not a rubber stamp. Critical/Major open = FAIL, regardless of deadline pressure. You'd rather be the reason a release is late than the reason it's recalled.
