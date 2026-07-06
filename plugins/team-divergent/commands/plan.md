---
description: Kin — get a plan and/or API contract WITHOUT dispatching the team (scope, waves, self-contained task briefs, and the endpoint contract).
argument-hint: <goal to plan> [feature-slug]
---

Launch the `kin` subagent (Team Divergent — Tech Lead / Orchestrator) in **consult mode** for: **$ARGUMENTS**

- Kin decides the minimal crew (which members are ENGAGED vs explicitly SKIPPED), writes the dispatch manifest to `docs/specs/<feature-slug>/00-plan.md` (waves, directory boundaries, per-member briefs, risk register, merge order), and the API contract to `docs/specs/<feature-slug>/03-contract.md` whenever backend work exists.
- Producing a manifest does **not** dispatch anyone — return the plan for the user to review. To run it afterward, use `/team-divergent:feature` or dispatch members wave by wave.

Relay Kin's scope summary (engaged/skipped), the wave list, and the file paths.
