# Work Organization

The three branches are **Work Types**, **Work Flows**, and **Backlog.md Conventions**.

## Work Types

Choose one primary type based on the promised outcome. Capture useful topics
with labels and urgency with priority. Split out a change when it is worth
delivering and verifying on its own.

| Type | Intake and triage | Example journey |
| --- | --- | --- |
| Bug | Establish the expected and observed behavior. Capture a reproduction or evidence of the failure. | A blank title is accepted. Reproduce the request, add a regression test, and merge the verified fix. |
| Feature | Describe the desired behavior and write acceptance criteria. | A client needs pagination. Agree on the page contract, implement it, test it, and merge. |
| Maintenance | Explain why the work matters and what must keep working. | A Drizzle update is due. Check the upgrade, exercise migrations with fresh and existing databases, and merge. |
| Spike | State the question, timebox, and decision or evidence to deliver. | Compare pagination strategies. Record evidence and a recommendation, and link delivery work when appropriate. |

A Bug restores an established expectation. A Feature adds or changes an
expectation. A confirmed Bug can include routine debugging. Link a Spike when
the investigation needs its own timebox and decision.

Use `Maintenance` with the `refactor` label for a standalone refactor.
A refactor that is part of delivering pagination can sit in the Feature task.

## Work Flows

### Intake and triage

The **Draft Pool** lives under `backlog/drafts/` and the **Task Pool** under
`backlog/tasks/` in the designated Backlog.md checkout. GitHub issues, direct
requests and design conversations, and discoveries during work are intake
channels. When capture runs, it records the report and source in the Draft
Pool. A source already captured can add evidence to its existing draft or task.

#### Capture contract

A draft requires a title, a description of the report, and its source. Use
a stable source reference when available, or record where the report came
from. Include relevant evidence when it is available.

#### Triage contract

Read the draft and search for related work as soon as there is enough
context to identify it. Record the result for the applicable outcome.

| Outcome | Required record |
| --- | --- |
| Needs context | On the draft, record what is missing, why it matters, and the next action to get it. The report stays in the Draft Pool. |
| Duplicate | On the draft, record the canonical task ID. Add the source and any new evidence to that task, then resolve the draft. |
| Promote | Create one canonical task that meets the promotion contract and carries forward the source and relevant evidence. |

Agents apply project rules for routine classification, deduplication, and
readiness. Maintainers set priority policy and resolve product trade-offs.

#### Promotion contract

A task requires a primary work type, intended outcome, scope, how to verify
the outcome, and its source. Include the [evidence for its work type](#work-types).
If a dependency is unresolved, record the task as blocked, link the dependency,
and state what will unblock it. A task is Ready when blockers to starting are
resolved.

### From design to tasks

A work package keeps its artifacts together under `docs/work/<package>/`.
The maintainer leads discovery with `grill-with-docs`. The project-owned
[`design-to-prd` skill](../skills/design-to-prd/SKILL.md) captures the agreed
outcome, scope, and constraints in `PRD.md`. Record which revision the
maintainer approves. That approval authorizes routine work within the PRD's
scope. Escalate a serious issue that needs the maintainer's judgment or a
change to that scope. Release actions follow the safeguards in [AGENTS.md](../AGENTS.md).

| Phase | Input and skills | Output |
| --- | --- | --- |
| Specify | Approved `PRD.md`, `spec-driven-development`, and `api-and-interface-design` for affected interfaces | `SPEC.md` with observable behavior, contracts, and acceptance criteria. |
| Plan | Reviewed `SPEC.md` and `planning-and-task-breakdown` | `PLAN.md` for the approach and `TODO.md` for individual items, each with a stable ID, outcome, acceptance criteria, verification, and dependencies. |
| Capture and promote | Reviewed plan and task breakdown, using the intake contracts above | One Backlog.md draft per item, then a canonical task for each eligible draft. |

Project phase instructions use automated review at Addy Osmani's spec and
plan approval points. Progress through specification, planning, and task
creation follows the approved PRD and the [Closed-Loop Review
rules](template-design.md#closed-loop-review).

When recording a planned item, use the package path and item ID to find
existing records on a repeat run. Link the task to the relevant specification
and plan sections. Map dependencies between planning items to their canonical
task IDs before marking a task Ready. Backlog.md records ownership and
progress. `TODO.md` preserves the reviewed breakdown. Use a Backlog.md
milestone when the tasks share a delivery checkpoint worth tracking, and
labels for categories that span work packages.

### Pickup

1. **Filter.** Consider unowned Ready tasks and interrupted tasks whose
   ownership has been released. The agent must be able to take the next
   action and any blockers to that action must be resolved. On a planned
   pause, the owner updates the shared task before releasing it. After an
   abrupt interruption, the maintainer or designated coordinator can release
   ownership. A period without updates leaves the claim in place.
2. **Rank.** Take the highest-priority eligible task. At equal priority,
   prefer interrupted work and then the oldest task. A direct assignment
   can select a specific eligible task.
3. **Claim.** The designated coordinator rechecks eligibility and records one
   owner in one serialized operation against the shared task record. Start
   work after the claim is confirmed. If another agent takes the task, refresh
   the candidates. An interrupted agent checks current ownership before
   resuming.
4. **Verify where to start.** Read the task's phase and next action, then
   compare them with the linked artifacts. For code changes, check the
   branch, commit, PR state, and the revision the recorded checks covered.
   Update a stale record from available evidence. If local-only work is
   missing, record the gap and continue from the latest shared checkpoint
   when the next action is clear.

### During work and at completion

The Backlog.md task is the living record of the work. Each work phase has
expected updates. Keep the current phase, completed work, evidence, blockers,
and next action visible on the task. Link durable work artifacts and add
comments when a phase needs context or a decision needs explaining.

Update the record as work progresses and before moving to the next phase. A
replacement agent can use the shared task and its linked artifacts to resume
without a separate handoff conversation.

Record actionable discoveries outside the task's scope as follow-up work.
If a discovery prevents the current task from completing, record it as a
blocker on that task.

Close a change task after verification and merge. When the acceptance criteria
require deployment, close it after deployment and a live check. A Spike
completes with its evidence and recommendation recorded.

### Internal improvements

When an agent finds actionable friction in the repository or its workflow,
file a separate Backlog.md task with the `internal-improvement` label and
one of the four work types. The improvement task holds the details and must
point to the task where the agent found the issue.

1. Search for an existing improvement first. When one exists, add the origin
   task reference and any new evidence to it.
2. For a new task, record the problem and impact, evidence, a proposed fix or
   investigation question, and how to verify the result. Add the origin task
   reference. A short link from the origin task gives the next agent a way
   to find the follow-up.
3. Triage the improvement through the ordinary backlog. When a fix is
   delivered, repeat the original failing or confusing step and record the
   result on the improvement task.

**Example** — While working on `TASK-42` (Add Todo pagination), an agent
follows the README in a fresh checkout. The API starts but sign-up fails
because the setup instructions skipped database migrations.

| Field | Example record |
| --- | --- |
| Improvement task | `TASK-57` — Fix the fresh-checkout setup instructions |
| Work type and label | Bug · `internal-improvement` |
| Origin | `TASK-42` |
| Evidence | A clean setup produces a missing-table error when signing up. |
| Proposed fix | Add the migration step to the README quickstart. |
| Verification | Follow the revised README from a fresh checkout and successfully sign up. |

The first agent runs migrations locally and completes pagination. The next
agent takes `TASK-57`, fixes the README, verifies the clean setup, and
closes the Bug after merge. The Continuous Improvement and Self-Healing
milestone will use these labeled tasks to identify recurring problems.

## Backlog.md Conventions

Decisions still open include the workflow states, how to record and confirm
shared claims, and the concrete representation of type, priority, references,
blockers, and the `internal-improvement` label. See the
[handoff](HANDOFF.md) for where to resume.
