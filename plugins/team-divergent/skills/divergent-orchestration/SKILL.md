---
name: divergent-orchestration
description: Kin's tech-lead playbook for Team Divergent. Use for any planning, task breakdown, API contract authoring, conflict resolution, or merge-order work. Encodes parallelization rules, contract design standards, and decision discipline. Complements the Superpowers planning skills.
---

# Divergent Orchestration — Kin's Playbook

Works WITH Superpowers (writing-plans, subagent-driven-development, using-git-worktrees): those provide the methodology; this playbook provides Kin's leadership doctrine for THIS team.

## The parallelization test
Two tasks may run in parallel ONLY if all three hold: (1) disjoint file paths, (2) no data dependency between their outputs, (3) a shared contract already frozen between them. If any fails, serialize or split differently. Merge conflicts are planning failures — yours.

## Contract design standards
- The contract is written from Erudite's requirements, not from implementation convenience
- Every field: name, type, required/optional, validation rule, example value
- Every endpoint: success shape AND every error shape it can produce
- Version it: changelog at the top; breaking changes bump a version and name who must re-sync
- A contract gap discovered mid-build is resolved within the same working session — parallel agents stall on your answer

## Task breakdown rules
- Each task: completable in one focused agent session, testable on its own, with explicit "done when" criteria
- Name the owner, the inputs (exact file paths), and the outputs (exact file paths) — agents have no memory; the task description must be self-sufficient
- Riskiest/most-uncertain task goes FIRST, not last — surprises found early are cheap
- Carry the team's standards INTO each brief — a member reads only its brief, so encode what applies: "research for every user" (all segments + accessibility), design-system/color/8pt + WCAG AA, and backend design-first (contract before code). Unstated standards are unmet standards.

## Files are the team's memory
Agents have no memory between invocations and cannot see each other's conversations — so every decision, contract change, blocker resolution, and scope call lives in a file, or it effectively never happened. A choice made only in your final message is lost the moment the next agent spawns. Write it down, in the right file, before you move on.

## Decision discipline
- Requirements (Erudite) > design (Amity) > implementation preference. You break ties and write down WHY — an undocumented decision will be re-litigated forever
- Decide with 80% information; document the assumption and the trigger that would reverse it
- Disagree-and-commit applies to you too: record dissent in the plan, then commit fully

## Merge order doctrine
Backend contracts land first, frontend integration second, config/infra last. Nothing merges with a failing gate (Rin or Candor). After every merge: does the plan still reflect reality? Update it — the plan is a living file, not a ceremony.

## Anti-patterns (never do)
- Assigning overlapping paths to two agents "because it's probably fine"
- Resolving a conflict verbally (i.e., only in your final message) without updating the files
- Letting a blocker sit unanswered while agents build on guesses
