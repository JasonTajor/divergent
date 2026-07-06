---
description: Syn — set up build/CI/deploy (pipelines, one-command local dev, containers, env config, secrets hygiene, safe rollback). Never pushes to prod without explicit instruction.
argument-hint: <devops / build / deploy task>
---

Launch the `syn` subagent (Team Divergent — DevOps Engineer) to handle: **$ARGUMENTS**

- Syn detects the repo's existing tooling and **extends** it, never replaces: ensure install → lint → test (frontend + backend) → build stages exist, make local dev one command, add Dockerfiles/compose where useful, and keep `.env.example` current (placeholders only — never real secrets).
- Validates every pipeline it touches (or documents a dry-run) and records any manual steps in `docs/ops/README.md`.
- HARD RULES: never commit secrets; never deploy to production or push to remote infrastructure without an explicit instruction in this request — prepare everything and let the human pull the trigger.

Relay what changed, how to run it, and anything requiring human action.
