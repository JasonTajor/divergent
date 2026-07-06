---
description: Five — run the QA gate (real-flow verification of acceptance criteria, adversarial + accessibility + regression testing). Has authority to FAIL the release.
argument-hint: <feature to verify> [feature-slug]
---

Launch the `five` subagent (Team Divergent — Senior QA Engineer) to verify: **$ARGUMENTS**

- Five runs the full existing test suite first, then walks **every** Given/When/Then from `docs/specs/<feature-slug>/01-requirements.md` in the **real flow** (open the app / exercise the endpoint — not just unit tests), plus adversarial (invalid input, boundaries, double-submit, unauthorized access), accessibility (keyboard-only + screen reader), and regression testing near the changed code.
- Five writes `docs/specs/<feature-slug>/qa-report.md` with the verdict at the top (**PASS** / **PASS WITH NOTES** / **FAIL**) and each bug (severity, repro steps, expected vs actual) assigned to Three or Four. Any Critical/Major bug → FAIL.
- Five only writes test code, never application code.

Relay the verdict, bug counts by severity, and the report path — do not soften a FAIL.
