# Team Divergent — Parallel Development Pipeline

**Divergent**: eight veteran Claude Code subagents that work as one software team,
with a dispatcher protocol so you can run the full pipeline, call any member solo,
or just tell Kin what you want and approve his dispatch plan.
Each member carries a personal playbook skill (bundled in `.claude/skills/`)
plus a power-tool skill from the ecosystem.

**Repo layout** (drop-in for any project):
```
CLAUDE.md                     <- dispatcher protocol (main session reads this)
.claude/agents/*.md           <- the 8 members
.claude/skills/divergent-*/   <- the 8 personal playbooks
```
Drop the `.claude/agents/` folder into the root of any project. The team is
stack-agnostic: every agent detects and follows your repo's existing
conventions, and Kin picks the stack on greenfield projects.

## The Team

Every member is a 20–30 year veteran with a personal playbook (bundled) and a power tool (installed):

| Member | Role | Personal playbook (bundled) | Power tool (install) |
|--------|------|-----------------------------|----------------------|
| **One**   | Senior UX Analyst, 25+ yrs | `divergent-research` | Superpowers → brainstorming |
| **Two**   | Senior UI/UX Designer, 25+ yrs | `divergent-design` | Impeccable (design side) |
| **Three** | Senior Frontend Engineer, 20+ yrs | `divergent-frontend` | Impeccable (execution side) |
| **Four**  | Senior Backend Developer, ~30 yrs | `divergent-backend` | Superpowers → TDD + systematic-debugging |
| **Five**  | Senior QA, 25+ yrs | `divergent-qa` | webapp-testing (official Anthropic) |
| **Kin**   | Tech Lead / Orchestrator, 30 yrs | `divergent-orchestration` | Superpowers → writing-plans + subagents + worktrees |
| **Syn**   | DevOps Engineer, 25+ yrs | `divergent-devops` | (infra-specific skills as needed: Helm, AWS CDK...) |
| **Rin**   | Security & Code Reviewer, 25+ yrs | `divergent-security` | /security-review (built-in) + Trail of Bits (optional) |

All eight playbooks ship in this zip under `.claude/skills/divergent-*/SKILL.md` —
they encode each member's craft standards, checklists, and anti-patterns. Edit them
to tune your team's standards; every member re-reads their playbook on every task.

## Setup

1. Get the team into your project root — either extract this zip there, or
   once you've pushed this to GitHub:
   ```
   git clone https://github.com/<you>/divergent-team.git /tmp/divergent
   cp -r /tmp/divergent/.claude /tmp/divergent/CLAUDE.md your-project/
   ```
   (`.claude/` and `CLAUDE.md` must sit at your project's root. If your project
   already has a CLAUDE.md, append the Dispatcher Protocol section to it.)
   Restart Claude Code in that project; run `/agents` to confirm all 8 loaded.
2. Install **Impeccable** (Two + Three):
   ```
   npx impeccable install
   ```
   then run `/impeccable init` once inside Claude Code to save your design context.
3. Install **Superpowers** (One, Four, Kin):
   ```
   /plugin marketplace add obra/superpowers-marketplace
   /plugin install superpowers@superpowers-marketplace
   ```
4. Install **webapp-testing** (Five) — requires Playwright locally:
   ```
   /plugin marketplace add anthropics/skills
   ```
   then install the webapp-testing skill from that marketplace.
5. **Nothing to install** for Rin (`/security-review` ships with Claude Code)
   or Syn (his playbook is bundled). Optional hardening: Trail of Bits security
   skills for Rin on security-sensitive projects.

Every agent degrades gracefully: if its skill isn't installed, it reports the
gap and applies the same discipline manually instead of silently skipping it.

## The Pipeline

```
One ──▶ Two ──▶ Kin ──▶ ┌─ Three ─┐ ──▶ Rin ──▶ Five ──▶ Syn
(research) (design) (plan+       │  (parallel) │  (review)  (QA gate)  (ship)
                     contract)   └─ Four  ──┘
```

Three and Four run **in parallel** — both build against Kin's API contract
(`docs/specs/<feature>/03-contract.md`), so neither waits for the other.

## How the team communicates

Agents have no memory between runs. All handoffs happen through files in
`docs/specs/<feature-slug>/`:

- `01-requirements.md` — One's spec
- `02-design.md` — Two's design
- `00-plan.md` — Kin's task breakdown + directory boundaries
- `03-contract.md` — Kin's API contract (the law for Three & Four)
- `blockers.md` — any agent logs gaps/problems here
- `review-rin.md` — Rin's security review
- `qa-report.md` — Five's verdict

## Control model

Every member is **standalone AND team-capable** — the same agent file handles
both. You hold direct control (call any member yourself, in any order, even in
parallel by asking the main session to launch several at once) or hand Kin
delegated control for a goal. Switch freely: start solo with Two, then hand the
result to Kin to distribute the rest.

## Three ways to use the team

**1. Kin-orchestrated (recommended)** — tell Kin the goal; he plans the minimal
crew and the main session dispatches it wave by wave, with your approval:
> Kin: I only need UI design and frontend for a pricing page — no backend.

Kin's manifest will engage Two → Three (mock-data mode), skip everyone else,
and you approve before anything is built.

**2. Solo members** — call any member directly for focused work:
> Three: fix the responsive layout on the navbar.
> Rin: security-review my uncommitted changes.
> One: research requirements for a referral program.

**3. Full pipeline** — for complete features, step-by-step with you as the gate:

## Example prompts (paste into Claude Code)

Start a feature:
> Use the **one** subagent to research and write requirements for: [describe your feature]. Use feature slug `my-feature`.

Then, step by step (approve each stage yourself):
> Use the **two** subagent to produce the design spec for `my-feature`.

> Use the **kin** subagent to write the plan and API contract for `my-feature`.

Parallel build (one prompt):
> Launch the **three** and **four** subagents in parallel to implement `my-feature` per the plan and contract.

Gate and ship:
> Use the **rin** subagent to review all changes for `my-feature`.

> Use the **five** subagent to run the QA gate for `my-feature`.

> Use the **syn** subagent to make sure CI runs lint, tests, and build for this repo.

## Tips

- **Pilot small first.** Run the full pipeline on a tiny feature (a login form,
  a settings page) to tune the prompts before real work.
- **You are the approval gate.** Review each agent's output file before
  dispatching the next stage — especially Kin's contract.
- **Blockers flow through Kin.** If Three or Four hit a contract gap, they log
  it to `blockers.md`; re-invoke Kin to resolve and version the contract.
- **Failed QA loops back.** Five's report assigns each bug to Three or Four;
  re-invoke the owner with the bug IDs, then re-run Rin (quick re-check) and Five.
