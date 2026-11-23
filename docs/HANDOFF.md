# Handoff — Bun TypeScript API template design

Date **2026-11-23** · Session focus **Agree on the template design and its agent workflows**

## Objective

Design a reusable GitHub template for small-to-medium JSON APIs. Work down
from one root to milestones and then decide the branches of each milestone
with the maintainer before scaffolding.

## Status / Resume Point

The maintainer approved the API stack, the first-layer milestones, the four
work types, the intake and triage rules, and the follow-up task protocol using
the `internal-improvement` label. The maintainer also approved the
[pickup rules](work-organization.md#pickup), the
[design-to-task workflow](work-organization.md#from-design-to-tasks), and
the [Todo pagination PRD revision 1](work/todo-pagination/PRD.md).

The [Todo pagination pilot](work/todo-pagination/PILOT.md) completed
specification and plan review, then promoted two tasks into an isolated
Backlog.md tracker. A repeat run left both task records byte-for-byte
identical. `TASK-1` is blocked until the runnable API Starter provides the
Todo route, session guard, validation response, and a passing baseline
`bun test src/todos`. `TASK-2`, the client guide, depends on `TASK-1`.

## Resume in Codex

On the laptop, open the repository and check out the published
`codex/design-handoff` branch.

```sh
git fetch origin
git switch codex/design-handoff
git pull --ff-only
```

Both design chats are waiting for the maintainer's answers to their first
round. Use the matching prompt below in either the existing chat or a fresh
chat opened in the repository. Record confirmed decisions in the project
docs as each round settles.

### A - 01 - API Starter - Arch

Read [AGENTS.md](../AGENTS.md) and the [template design](template-design.md).
The first `grill-with-docs` round proposed three choices for confirmation.

| Decision | Recommendation from the first round |
| --- | --- |
| App composition | Use a `createApp()` factory that composes feature plugins with explicit dependencies. A separate entry point opens the database and starts the listener. Tests can exercise the app without binding a port. |
| SQLite deployment | Start with one API instance on one host, using a SQLite file on a persistent local volume. The file survives redeploys. Host failure requires recovery, and multi-host replicas would need a different persistence model. |
| Migration ownership | Use Better Auth's CLI to generate its Drizzle schema. Use Drizzle Kit to generate and apply one versioned migration history for both app and auth tables. This keeps the Todo owner foreign key and its referenced auth table in one migration order. |

After agreement, record the decisions and use
[`design-to-prd`](../skills/design-to-prd/SKILL.md) to draft the API Starter
PRD for maintainer approval. After PRD approval, verify the
[toolchain and direct dependencies](template-design.md#stack-and-dependencies)
together. Plan independent setup slices where possible. The HTTP slice
starts with Elysia, Zod, and Neverthrow. The persistence slice starts with
Bun SQLite, Drizzle, and Better Auth. Scaffolding requires maintainer
agreement under [AGENTS.md](../AGENTS.md).

> Resume the API Starter architecture session from docs/HANDOFF.md using
> grill-with-docs. Bring back the three pending recommendations on Elysia
> composition, SQLite deployment, and migration ownership so I can confirm
> or change them. Then record what I confirm and draft the PRD for approval.

### B - 01 - Agent Workflows - Task Phases

Read [AGENTS.md](../AGENTS.md), [Work Organization](work-organization.md),
and the [pilot record](work/todo-pagination/PILOT.md). The first
`grill-with-docs` round used `TASK-1` and `TASK-2` to propose these
checkpoints. Each proposal is awaiting the maintainer's answer.

| Question | Recommendation from the first round |
| --- | --- |
| Q1 — Pickup | The coordinator confirms the claim. The owner reads the task, spec, plan, and any branch or PR, then records prerequisite evidence, the starting revision, and the first action. Use `git-workflow-and-versioning` for code work. |
| Q2 — Implementation | The owner works in slices from the reviewed plan and acceptance criteria. Use `incremental-implementation`, `test-driven-development` for behavior changes, and `documentation-and-adrs` for guide work. Record completed slices, shared revisions, checks, blockers, and the next action. Advance when the candidate covers the task scope. |
| Q3 — Verification | The owner checks the candidate against the acceptance criteria and records its revision, procedures, results, and evidence. Use `test-driven-development` for behavior checks and `debugging-and-error-recovery` for unexpected failures. Advance when the required checks pass. |
| Q4 — Review | The owner retains the claim while an independent reviewer reads the diff, criteria, and evidence. Use `code-review-and-quality` for code. Record the review link, reviewed revision, open findings, and next action. Advance when review clears and affected checks pass. |
| Q5 — Merge | The owner uses `git-workflow-and-versioning` to confirm that the current PR meets review, CI, and approval requirements. An authorized merger squash-merges to `main`. Record the PR and resulting commit. |
| Q6 — Closure | The owner checks the outcome against the acceptance criteria, verification, merge, and dependencies. Record final evidence, follow-ups, and dependencies satisfied. Close change tasks after verified merge and any required authorized deployment and live check. Use `shipping-and-launch` for release work. Close a Spike with its evidence and recommendation. |
| Q7 — Interruption and resume | Before a planned release, record the current phase, latest shared revision, passed checks, remaining work, blockers, and next action. After an abrupt interruption, apply the agreed claim release rules. The replacement confirms ownership and reconciles the task against shared evidence before resuming. |

Use the pilot to test the boundaries. If `TASK-1` was merged during an
interruption, reconcile the merge and resume at closure. If the PR revision
changed, go back to the earliest affected check or review. Closing
`TASK-1` satisfies the client guide's dependency.

Use [Work Organization](work-organization.md) to record decisions as the
maintainer confirms them. Give the routing skill and specialist profiles
their own [Closed-Loop Review](template-design.md#closed-loop-review) design
session. Use **Backlog.md Conventions** to map agreed workflow decisions to
actual statuses, fields, and commands. Dispatcher authorization and
scheduling belong to [Task Dispatcher](template-design.md#task-dispatcher).

> Resume Agentic Workflow Design from docs/HANDOFF.md using grill-with-docs.
> The Todo pagination design-to-task pilot is complete. Bring back Q1–Q7
> from the pending pickup-to-closure round so I can confirm or change them.
> Record confirmed decisions as we go. Keep the review router and
> specialist profiles in the Closed-Loop Review milestone.

## Artifacts Touched

- [README.md](../README.md) — project overview and entry point.
- [AGENTS.md](../AGENTS.md) — design workflow, skills, and operational constraints.
- [GLOSSARY.md](../GLOSSARY.md) — agreed terms and their meanings.
- [docs/template-design.md](template-design.md) — accepted milestone tree,
  dependency inventory, account scope, and Todo API design.
- [docs/work-organization.md](work-organization.md) — accepted classification,
  intake, pickup, completion, and internal-improvement workflow with an
  end-to-end example.
- [Todo pagination pilot](work/todo-pagination/PILOT.md) — reviewed work
  package and canonical task records in an isolated tracker.
- [Opening draft](superpowers/plans/2026-11-23-bun-ts-github-template.md) —
  initial framing and earlier design notes.

## Pitfalls & Discoveries

- The opening draft describes a general-purpose starter and has an early
  feedback protocol. Use the current design and workflow docs for the
  approved API scope and linked improvement tasks.
- Verify the exact package pins and the toolchain together before writing
  starter code. The [dependency inventory](template-design.md#stack-and-dependencies)
  records the baseline, the candidate compiler patch, and the local Bun version.
- Verify Backlog.md's current CLI and configuration before mapping the
  agreed readiness rules to fields or commands. The playbook's readiness
  includes scope and verification as well as dependencies.
- Backlog.md 1.57.4's task edit locks apply per checkout. Its web API can
  check the expected task revision within one checkout. Coordinate claims
  across agents through an authoritative task record. See the
  [task lock](https://github.com/MrLesk/Backlog.md/blob/v1.57.4/src/file-system/operations.ts#L701-L722)
  and [revision check](https://github.com/MrLesk/Backlog.md/blob/v1.57.4/src/core/backlog.ts#L2887-L2918).

## Constraints

Follow [AGENTS.md](../AGENTS.md) for safety checkpoints, Git, and verification.
Continue the design discussion before scaffolding or enabling GitHub's
template setting. The maintainer prefers concise rounds with concrete
examples.

## Project Skills

Codex reads the third-party skills from [`.agents/skills/`](../.agents/skills/).
The [skills lockfile](../skills-lock.json) records their upstream sources and
revisions. The project owns [`design-to-prd`](../skills/design-to-prd/SKILL.md).

- Use Matt Pocock’s `grill-with-docs` for design sessions. It combines
  `grilling` for the interview and `domain-modeling` for the glossary and
  agreed decisions.
- Use Addy Osmani’s `spec-driven-development`, `api-and-interface-design`,
  and `planning-and-task-breakdown` for specifications, contracts, and plans.
- Use `incremental-implementation` and `test-driven-development` to build
  and verify changes in slices. Use `debugging-and-error-recovery` to
  investigate unexpected failures.
- Use `code-review-and-quality` for review, `documentation-and-adrs` for
  docs and decisions, and `git-workflow-and-versioning` for PR delivery.
  Use `ci-cd-and-automation` for CI checks and `shipping-and-launch` when
  release work is in scope. Verify the outcome before closing the task.

The skills aid the agent workflow. The linked project docs carry the
decisions so the design can be resumed with another agent or toolchain.

## Later Design Questions

- What triggers capture for each intake channel, and what triggers triage
  for a new or updated draft?
- How should Closed-Loop Review route reviews and define specialist
  contracts under the agreed finding grades and ten-round cap?
- Which Backlog.md states and fields should implement the workflow?
- Should projects opt into triage or implementation dispatchers? What
  authorizes unattended dispatch, and should dispatchers poll the pools or
  respond to events?
- How will the template prove that recurring friction was addressed, and
  which limited recovery actions are candidates for automation? Save the
  deep dive for Continuous Improvement and Self-Healing.
