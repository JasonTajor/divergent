# Team Divergent — a full software team as a Claude Code plugin

**Divergent** is eight veteran Claude Code subagents that work as one software
team, plus a dispatcher protocol so you can run the full pipeline, call any
member solo, or just tell **Kin** what you want and approve his dispatch plan.
Each member carries a personal playbook skill, and the design members build
against the bundled **Divergent Design System** (`DIVERGENT-DESIGN.md`).
Distributed as an installable plugin through this repo's built-in
**marketplace** (Claude Code's package manager) — install it once and use the
team in any project.

The five production members are named for the Divergent factions, aligned to
each role's nature: **Erudite** (research — intellectual), **Amity** (design —
peaceful), **Dauntless** (frontend — brave), **Abnegation** (backend —
selfless), and **Candor** (QA — honest). **Kin**, **Rin**, and **Syn** round out
orchestration, security review, and DevOps.

## Install (terminal Claude Code)

Inside a Claude Code session, run:

```
/plugin marketplace add JasonTajor/divergent
/plugin install team-divergent@divergent
```

That's it. `marketplace add` registers this repo as a package source (the
"package manager"); `install` pulls the `team-divergent` plugin from it.
Restart if prompted, then run `/agents` — you should see all eight members
namespaced as `team-divergent:erudite … team-divergent:rin`.

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
  > `team-divergent:dauntless` fix the responsive layout on the navbar
  > Rin, security-review my uncommitted changes
  > Erudite: research requirements for a referral program

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

## Commands

The plugin ships slash commands mapped to the team structure. Each one launches
the right member with your input and drops its outputs in the standard file
locations. Type the command, then your request.

| Command | Member | What it does |
|---------|--------|--------------|
| `/team-divergent:feature <goal>` | Kin → crew | Full pipeline: Kin plans + writes the contract, you approve, the session dispatches the crew in parallel waves through review, QA, and ship. |
| `/team-divergent:research <idea>` | Erudite | Writes a testable requirements spec — user segments (incl. accessibility), jobs-to-be-done, flows, numbered FRs, Given/When/Then acceptance criteria → `01-requirements.md`. |
| `/team-divergent:design <feature>` | Amity | Produces the UI/UX design spec against the Divergent Design System — resolved Decision Gate, tokens, 8pt grid, unified components, every interaction state → `02-design.md`. |
| `/team-divergent:frontend <task>` | Dauntless | Implements UI in code — enforces the Divergent Design System tokens, 8pt grid, one component system, all states + accessibility; runs lint/build/tests. |
| `/team-divergent:backend <task>` | Abnegation | Implements the server side — data-model-first, optimized up front, validated + authorized + tested. |
| `/team-divergent:plan <goal>` | Kin | Plan and/or API contract **without** dispatching anyone → `00-plan.md` (+ `03-contract.md`). Review it, then run `/team-divergent:feature`. |
| `/team-divergent:review [target]` | Rin | Security & code review of the diff — injection, auth/IDOR, secrets, data exposure, contract compliance. Read-only → `review-rin.md`. |
| `/team-divergent:qa <feature>` | Candor | QA gate — real-flow verification of acceptance criteria, adversarial + accessibility + regression. Can **FAIL** the release → `qa-report.md`. |
| `/team-divergent:devops <task>` | Syn | Build/CI/deploy — pipelines, one-command local dev, containers, env config, secrets hygiene, safe rollback. |
| `/team-divergent:divergent` | — | Loads the dispatcher protocol (solo vs orchestrated routing rules) into the current session. |

Handoffs still flow through `docs/specs/<feature-slug>/` (see below), so a spec
written by `/team-divergent:research` is picked up by `/team-divergent:design`,
and so on — the commands are just a faster way to invoke each stage.

### Quick-task commands

Focused, run-it-now commands for the everyday jobs. Each fires Dauntless or
Abnegation with a strong pre-baked brief and operates on what you name (or your
most recent changes if you name nothing). No spec or pipeline required.

**Frontend (Dauntless)**

| Command | What it does |
|---------|--------------|
| `/team-divergent:responsive` | Make a page/component responsive, mobile-first, on the 8pt grid — no overflow or horizontal scroll. |
| `/team-divergent:fix-spacing` | Snap all spacing to the 8pt grid via tokens; kill magic values and uneven gaps. |
| `/team-divergent:fix-colors` | Replace raw/random colors with harmony-based palette tokens; fix contrast; remove unmotivated gradients. |
| `/team-divergent:a11y` | Accessibility fix — semantic HTML, keyboard, focus, ARIA, contrast, reduced-motion (WCAG AA). |
| `/team-divergent:states` | Add the missing UI states (loading/empty/error/success/disabled) with real copy. |
| `/team-divergent:polish` | Presentation-only polish — hierarchy, alignment, typography; strips the generic "AI look." |

**Backend (Abnegation)**

| Command | What it does |
|---------|--------------|
| `/team-divergent:optimize` | Fix N+1s, missing indexes, `SELECT *`, unbounded queries — behavior unchanged. |
| `/team-divergent:secure-endpoint` | Enforce auth + per-resource ownership (fix IDOR), rate-limit, sanitize, consistent errors. |
| `/team-divergent:validate` | Add strict boundary validation (type/length/range/format) with the consistent error envelope. |
| `/team-divergent:paginate` | Add pagination + limits to a list endpoint, bounded at the DB, index-backed. |
| `/team-divergent:migration` | Write a safe, additive, reversible migration with DB-level constraints and a tested rollback. |

## The team

Every member is a 20–30 year veteran with a bundled personal playbook and an optional power tool:

| Member | Role | Personal playbook (bundled) | Power tool |
|--------|------|-----------------------------|------------|
| **Erudite** | Senior UX Analyst, 25+ yrs — research for *every* user (intellectual) | `divergent-research` | Superpowers → brainstorming |
| **Amity** | Senior UI/UX Designer, 25+ yrs — Apple HIG + Google Material, 8pt, harmony (peaceful) | `divergent-design` | Divergent Design System (`DIVERGENT-DESIGN.md`, bundled) |
| **Dauntless** | Senior Frontend Engineer, 20+ yrs — enforces the tokens, 8pt grid & one component system (brave) | `divergent-frontend` | Divergent Design System (`DIVERGENT-DESIGN.md`, bundled) |
| **Abnegation** | Senior Backend Developer & Architect, ~30 yrs — design-first, optimization up front (selfless) | `divergent-backend` | Superpowers → TDD + systematic-debugging |
| **Candor** | Senior QA, 25+ yrs — real-flow verification, accessibility as acceptance (honest) | `divergent-qa` | webapp-testing (official Anthropic) |
| **Kin**   | Tech Lead / Orchestrator, 30 yrs | `divergent-orchestration` | Superpowers → writing-plans + subagents + worktrees |
| **Syn**   | DevOps Engineer, 25+ yrs | `divergent-devops` | (infra-specific skills as needed: Helm, AWS CDK…) |
| **Rin**   | Security & Code Reviewer, 25+ yrs | `divergent-security` | /security-review (built-in) + Trail of Bits (optional) |

Every agent degrades gracefully: if its power tool isn't installed, it reports
the gap and applies the same discipline manually instead of silently skipping.

The **Divergent Design System** (`DIVERGENT-DESIGN.md`) ships with the plugin —
no external install. Amity and Dauntless resolve its Decision Gate with you, emit
its tokens, and obey its ten UX Laws. Optional power tools for the other members:

```
/plugin marketplace add obra/superpowers-marketplace     # Erudite, Abnegation, Kin
/plugin install superpowers@superpowers-marketplace
/plugin marketplace add anthropics/skills                # Candor (webapp-testing; needs Playwright)
```

## The pipeline

```
Erudite ──▶ Amity ──▶ Kin ──▶ ┌─ Dauntless  ─┐ ──▶ Rin ──▶ Candor ──▶ Syn
(research) (design)  (plan+   │ (parallel)   │  (review)  (QA gate)  (ship)
                     contract)└─ Abnegation ─┘
```

Dauntless and Abnegation run **in parallel** — both build against Kin's API
contract (`docs/specs/<feature>/03-contract.md`), so neither waits for the other.

## How the team communicates

Agents have no memory between runs. All handoffs happen through files in
`docs/specs/<feature-slug>/`:

- `01-requirements.md` — Erudite's spec
- `02-design.md` — Amity's design
- `00-plan.md` — Kin's task breakdown + directory boundaries
- `03-contract.md` — Kin's API contract (the law for Dauntless & Abnegation)
- `blockers.md` — any agent logs gaps/problems here
- `review-rin.md` — Rin's security review
- `qa-report.md` — Candor's verdict

## Repo layout (this is both the marketplace and the plugin)

```
.claude-plugin/marketplace.json          <- the package source ("package manager")
plugins/team-divergent/
  .claude-plugin/plugin.json             <- the plugin manifest
  DIVERGENT-DESIGN.md                     <- the Divergent Design System (token + UX contract)
  agents/*.md                            <- the 8 members
  skills/divergent-*/SKILL.md            <- the 8 personal playbooks
  commands/*.md                          <- slash commands (feature, research, design,
                                            frontend, backend, plan, review, qa, devops,
                                            divergent)
```

Tune the team by editing the playbooks under `skills/divergent-*/` and the
design contract in `DIVERGENT-DESIGN.md` — every member re-reads its playbook on
every task. Bump `version` in `plugins/team-divergent/.claude-plugin/plugin.json`
when you publish changes so installers get the update.

## Manual install (no marketplace)

Prefer to drop the team straight into one project instead of installing the
plugin? Copy the members and playbooks into that project's `.claude/`:

```
git clone https://github.com/JasonTajor/divergent.git /tmp/divergent
mkdir -p your-project/.claude
cp -r /tmp/divergent/plugins/team-divergent/agents your-project/.claude/agents
cp -r /tmp/divergent/plugins/team-divergent/skills  your-project/.claude/skills
cp /tmp/divergent/plugins/team-divergent/DIVERGENT-DESIGN.md your-project/DIVERGENT-DESIGN.md
cp /tmp/divergent/plugins/team-divergent/commands/divergent.md your-project/CLAUDE.md
```

Restart Claude Code in that project and run `/agents` to confirm all 8 loaded.

## Tips

- **Pilot small first.** Run the full pipeline on a tiny feature (a login form,
  a settings page) to tune the prompts before real work.
- **You are the approval gate.** Review each agent's output file before
  dispatching the next stage — especially Kin's contract.
- **Blockers flow through Kin.** If Dauntless or Abnegation hit a contract gap,
  they log it to `blockers.md`; re-invoke Kin to resolve and version the contract.
- **Failed QA loops back.** Candor's report assigns each bug to Dauntless or
  Abnegation; re-invoke the owner with the bug IDs, then re-run Rin and Candor.
