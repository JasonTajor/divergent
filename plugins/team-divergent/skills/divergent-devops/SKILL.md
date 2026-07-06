---
name: divergent-devops
description: The team's CI/CD and operations standards. Use for any pipeline, build tooling, environment, container, or deployment work. Encodes stage order, secrets hygiene, reproducibility, and rollback rules that apply regardless of stack or cloud.
---

# Divergent DevOps — Syn's Playbook

Stack-agnostic operating standards for all pipeline and infrastructure work in this project. Where the project's infrastructure has a dedicated community skill (Helm, AWS CDK, etc.), use it IN ADDITION to these standards.

## 1. Pipeline stage order (never skip, never reorder)
install → lint → test (all suites) → build → (optional) package → deploy-to-staging → smoke test → deploy-to-prod (human-triggered)
- Every stage fails fast and fails loud. A yellow/flaky stage is a red stage.
- Cache dependencies keyed by lockfile hash; never cache test results.

## 2. Reproducibility rules
- Pin versions: language runtime, base images, and dependencies via lockfiles. "latest" is forbidden in anything that ships.
- One-command local dev (`dev` script or compose file). If a new engineer can't run the project in one command, that's a bug — yours.
- The CI environment and local environment run the SAME commands. No CI-only magic.

## 3. Secrets hygiene (zero tolerance)
- Secrets live in the platform's secret store / environment — never in code, config files, images, or logs.
- Every new variable gets a placeholder line in `.env.example` with a one-line comment.
- Grep the diff for high-entropy strings and known key prefixes before finishing. Found a committed secret? Stop, flag to Rin, and treat it as compromised (rotation required), not just deleted.

## 4. Rollback readiness
- Every deploy must be reversible: keep the previous artifact/image deployable, migrations backward-compatible for at least one release.
- Document the exact rollback command/steps in `docs/ops/README.md` BEFORE the first deploy, not after the first incident.
- A deploy without a tested rollback path is not ready to ship — reversibility is a precondition, not a follow-up.

## 4b. Human-gated actions (hard line)
- Production and remote-infra pushes are triggered by a human, never autonomously. Prepare everything — build, stage, smoke test, rollback command — then hand the trigger to a person unless the prompt explicitly instructs the push.
- Staging is yours to exercise freely; prod is the human's to authorize.

## 5. Observability minimum
- Build logs answer "what version, what commit, what triggered it" at a glance.
- The app exposes a health/readiness endpoint (ask Four if missing); the pipeline's smoke test hits it.

## 6. Definition of quality for any pipeline change
- Runs green end-to-end (or documented dry-run where local execution is impossible)
- A human can understand each stage's purpose from its name alone
- No secrets introduced; `.env.example` current
- Rollback path documented
