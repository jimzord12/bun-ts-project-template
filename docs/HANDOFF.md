# Handoff — Bun TypeScript API template design

Date **2026-11-23** · Session focus **Agree on the template design and its agent workflows**

## Objective

Design a reusable GitHub template for small-to-medium JSON APIs. Work down
from one root to milestones and then decide the branches of each milestone
with the maintainer before scaffolding.

## Status / Resume Point

The maintainer approved the API stack, the seven first-layer milestones, the
four work types, the intake and triage rules, and the follow-up task protocol
using the `internal-improvement` label. The design is at the **Work
Organization** milestone. The repository is at a documentation checkpoint.

Open the [current design](template-design.md) and
[Work Organization](work-organization.md). Use them to continue the interview
at the next decision round.

## Artifacts Touched

- [README.md](../README.md) — project overview and entry point.
- [AGENTS.md](../AGENTS.md) — design workflow, skills, and operational constraints.
- [GLOSSARY.md](../GLOSSARY.md) — agreed terms and their meanings.
- [docs/template-design.md](template-design.md) — accepted milestone tree,
  dependency inventory, account scope, and Todo API design.
- [docs/work-organization.md](work-organization.md) — accepted classification,
  intake, completion, and internal-improvement workflow with an end-to-end
  example.
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

## Constraints

Follow [AGENTS.md](../AGENTS.md) for safety checkpoints, Git, and verification.
Continue the design discussion before scaffolding or enabling GitHub's
template setting. The maintainer prefers concise rounds with concrete
examples.

## Next Session Focus

Resume **Work Organization → Work Flows** with the next design round. Walk a
ready task through pickup, ownership, handoff or interruption, review, and
closure. Include a two-agent scenario so task assignment and recovery are
testable. Then use **Backlog.md Conventions** to map the agreed workflow to
actual statuses, dependencies, references, and labels. Confirm the remaining
Work Organization decisions before moving to another milestone.

Suggested opening request:

> Read docs/HANDOFF.md and resume the grill-with-docs design session from
> the next Work Organization round.

## Suggested Skills

- `grill-with-docs` — the requested design workflow. If the wrapper is
  unavailable, use `grilling` to run decision rounds and `domain-modeling`
  to update the glossary and capture decisions as they settle.
- `jz-load-handoff` — use if available to resume from this file in another
  Codex session. Reading the file directly also works.
- `writing-plans` — turn agreed requirements into implementation steps
  before scaffolding.
- `test-driven-development` and `systematic-debugging` — use for features
  and bug fixes once implementation starts, and for investigating unexpected
  failures.
- `requesting-code-review` and `verification-before-completion` — apply at
  review and delivery. Verify the actual output before making a completion claim.

The skills aid the agent workflow. The linked project docs carry the
decisions so the design can be resumed with another agent or toolchain.

## Open Questions

- How do agents claim a ready task, hand it off, and release ownership
  after an interruption?
- Which Backlog.md states and fields should implement the workflow?
- How will the template prove that recurring friction was addressed, and
  which limited recovery actions are candidates for automation? Save the
  deep dive for Continuous Improvement and Self-Healing.
