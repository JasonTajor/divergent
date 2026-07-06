# Team Divergent — a full software team as a Claude Code plugin

**Divergent** is eight veteran Claude Code subagents that work as one software
team, plus a dispatcher protocol so you can run the full pipeline, call any
member solo, or just tell **Kin** what you want and approve his dispatch plan.
Each member carries a personal playbook skill. Distributed as an installable
plugin through this repo's built-in **marketplace** (Claude Code's package
manager) — install it once and use the team in any project.

## Install (terminal Claude Code)

Inside a Claude Code session, run:

```
/plugin marketplace add JasonTajor/divergent
/plugin install team-divergent@divergent
```

That's it. `marketplace add` registers this repo as a package source (the
"package manager"); `install` pulls the `team-divergent` plugin from it.
Restart if prompted, then run `/agents` — you should see all eight members
namespaced as `team-divergent:one … team-divergent:rin`.

**Manage it like any package:**

```
/plugin list                         # what's installed / enabled
/plugin update team-divergent        # pull a newer version
/plugin disable team-divergent       # turn off without removing
/plugin uninstall team-divergent     # remove
/plugin marketplace update divergent # refresh the source
```

Prefer the shell? The same commands exist non-interactively:

```
claude plugin marketplace add JasonTajor/divergent
claude plugin install team-divergent@divergent
```

> **Note:** package name is `team-divergent`, marketplace name is `divergent`,
> so the install target is `team-divergent@divergent`.

## Use it

- **Solo — call any member by name** (they're fully standalone):
  > `team-divergent:three` fix the responsive layout on the navbar
  > Rin, security-review my uncommitted changes
  > One: research requirements for a referral program

- **Orchestrated — hand Kin a goal**, approve his plan, and the session
  dispatches the crew in parallel waves:
  > Kin: build a pricing page — UI design and frontend only, no backend.

- **Load the dispatcher protocol** into the current session any time with the
  bundled command:
  ```
  /team-divergent:divergent
  ```
  To have it apply automatically in a project, copy that protocol into the
  project's root `CLAUDE.md`.

## The team

Every member is a 20–30 year veteran with a bundled personal playbook and an optional power tool:

| Member | Role | Personal playbook (bundled) | Power tool (optional) |
|--------|------|-----------------------------|-----------------------|
| **One**   | Senior UX Analyst, 25+ yrs — research for *every* user | `divergent-research` | Superpowers → brainstorming |
| **Two**   | Senior UI/UX Designer, 25+ yrs — Apple HIG + Google Material, Color Wheel Theory, 8pt | `divergent-design` | Impeccable (design side) |
| **Three** | Senior Frontend Engineer, 20+ yrs — enforces the palette, 8pt grid & one component system | `divergent-frontend` | Impeccable (execution side) |
| **Four**  | Senior Backend Developer & Architect, ~30 yrs — design-first, optimization up front | `divergent-backend` | Superpowers → TDD + systematic-debugging |
| **Five**  | Senior QA, 25+ yrs — real-flow verification, accessibility as acceptance | `divergent-qa` | webapp-testing (official Anthropic) |
| **Kin**   | Tech Lead / Orchestrator, 30 yrs | `divergent-orchestration` | Superpowers → writing-plans + subagents + worktrees |
| **Syn**   | DevOps Engineer, 25+ yrs | `divergent-devops` | (infra-specific skills as needed: Helm, AWS CDK…) |
| **Rin**   | Security & Code Reviewer, 25+ yrs | `divergent-security` | /security-review (built-in) + Trail of Bits (optional) |

Every agent degrades gracefully: if its power tool isn't installed, it reports
the gap and applies the same discipline manually instead of silently skipping.
Optional power tools:

```
npx impeccable install                                   # Two + Three (then /impeccable init)
/plugin marketplace add obra/superpowers-marketplace     # One, Four, Kin
/plugin install superpowers@superpowers-marketplace
/plugin marketplace add anthropics/skills                # Five (webapp-testing; needs Playwright)
```

## What Team Divergent can do

Not just "eight agents" — a breakdown of the jobs the team actually does for you.
Each capability names the members that own it, so you know exactly who to call.

1. **Ship a whole feature end-to-end.** Research → design → API contract →
   parallel build → security review → QA gate → release prep, with you approving
   each stage. *(the full crew, orchestrated by Kin)*

2. **Turn a vague idea into a testable spec.** Interrogates assumptions, maps
   primary/alternative/failure user flows, and writes numbered, testable
   acceptance criteria — for *every* user, including accessibility and edge
   cases. *(One)*

3. **Design a real, non-generic UI system.** Apple-HIG + Google-Material
   thinking, a **Color Wheel Theory** palette (named scheme, one brand hue,
   60-30-10, OKLCH ramps — no random hex), an **8pt spatial grid**, and one
   unified component set. Kills the templated "AI look." *(Two)*

4. **Build the frontend to that spec — faithfully.** Enforces the palette, the
   8pt grid, and a single component library in code; implements every state
   (loading/empty/error/success/disabled) with real accessibility. *(Three)*

5. **Design a backend that won't need constant refactoring.** Data-model-first,
   access patterns and performance budgets decided up front, correct
   validation/authorization, transactions, and migrations. *(Four)*

6. **Guarantee frontend and backend agree.** A single API contract is the law
   both sides build against — so Three and Four work **in parallel** and still
   integrate cleanly. *(Kin authors it; Three + Four implement it)*

7. **Catch security holes before merge.** Injection, auth/authorization bypass,
   IDOR, SSRF, secrets, dependency risk, data exposure — read-only review that
   reports, never silently "fixes." *(Rin)*

8. **Prove the feature actually works.** Runs the real user flow (not just unit
   tests), adversarial and un-happy-path testing, accessibility checks, and
   regression — with authority to **FAIL** the release. *(Five)*

9. **Make it build, test, and ship repeatably.** CI/CD pipelines, one-command
   local dev, containers, environment config, secrets hygiene, and safe
   rollback. Never pushes to production without your say-so. *(Syn)*

10. **Work your way, and scale to the job.** Call any member **solo** for a
    focused task, or hand **Kin** a goal and approve a scoped plan (UI-only,
    backend-only, review-only…). Switch between the two mid-feature; the team
    only engages the members a task actually needs.

> **How to trigger any of these:** address a member by name for the solo jobs
> (2–9), or say `Kin: <goal>` for orchestrated jobs (1, 6, 10).

## The pipeline

```
One ──▶ Two ──▶ Kin ──▶ ┌─ Three ─┐ ──▶ Rin ──▶ Five ──▶ Syn
(research)(design)(plan+  │(parallel)│  (review)  (QA gate)  (ship)
                  contract)└─ Four  ─┘
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

## Repo layout (this is both the marketplace and the plugin)

```
.claude-plugin/marketplace.json          <- the package source ("package manager")
plugins/team-divergent/
  .claude-plugin/plugin.json             <- the plugin manifest
  agents/*.md                            <- the 8 members
  skills/divergent-*/SKILL.md            <- the 8 personal playbooks
  commands/divergent.md                  <- /team-divergent:divergent dispatcher protocol
```

Tune the team by editing the playbooks under `skills/divergent-*/` — every
member re-reads its playbook on every task. Bump `version` in
`plugins/team-divergent/.claude-plugin/plugin.json` when you publish changes so
installers get the update.

## Manual install (no marketplace)

Prefer to drop the team straight into one project instead of installing the
plugin? Copy the members and playbooks into that project's `.claude/`:

```
git clone https://github.com/JasonTajor/divergent.git /tmp/divergent
mkdir -p your-project/.claude
cp -r /tmp/divergent/plugins/team-divergent/agents your-project/.claude/agents
cp -r /tmp/divergent/plugins/team-divergent/skills  your-project/.claude/skills
cp /tmp/divergent/plugins/team-divergent/commands/divergent.md your-project/CLAUDE.md
```

Restart Claude Code in that project and run `/agents` to confirm all 8 loaded.

## Tips

- **Pilot small first.** Run the full pipeline on a tiny feature (a login form,
  a settings page) to tune the prompts before real work.
- **You are the approval gate.** Review each agent's output file before
  dispatching the next stage — especially Kin's contract.
- **Blockers flow through Kin.** If Three or Four hit a contract gap, they log
  it to `blockers.md`; re-invoke Kin to resolve and version the contract.
- **Failed QA loops back.** Five's report assigns each bug to Three or Four;
  re-invoke the owner with the bug IDs, then re-run Rin and Five.
