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

1. Accept reports from GitHub issues, direct requests, and discoveries during
   work. Give tracked work one canonical Backlog.md task and add source
   references when available.
2. Search for an existing task. Link duplicate reports to it and add any new
   evidence.
3. Classify the work and assess readiness. A task is ready to start when its
   outcome, scope, and verification are clear, the evidence for its type is
   present, and blockers to starting are resolved.
4. Keep an incomplete report in intake and record what evidence is missing.
   Keep a well-defined task with an unresolved dependency in the backlog as
   blocked. Link the dependency and state what will unblock it.
5. Agents apply project rules for routine classification, deduplication, and
   readiness. Maintainers set priority policy and resolve product trade-offs.

### During work and at completion

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

Decisions still open include the workflow states, ownership and handoffs
with multiple agents, and the concrete representation of type, priority,
references, blockers, and the `internal-improvement` label. See the
[handoff](HANDOFF.md) for where to resume.
