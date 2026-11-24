---
name: design-to-prd
description: Distill an agreed design conversation and its referenced decisions into a work package PRD for maintainer approval. Use when discovery has reached agreement and the maintainer wants to turn it into work.
---

# Design to PRD

Read the agreed design and the project documents it relies on. Preserve the
project's terminology and distinguish confirmed decisions from options and
assumptions. Cite durable source documents or summarize the conversation's
decisions when a source link is unavailable.

Use the supplied work-package folder or a descriptive slug under
`docs/work/`. Write a proposed `PRD.md` with the following content:

- The user or client need and the intended outcome.
- The scope, constraints, and behavior the change must preserve.
- Observable success criteria.
- Open decisions that affect approval. Identify technical choices the design
  leaves to specification.
- References to the agreed design and evidence.

Use only sections that carry information. Surface unresolved product
choices for the maintainer. Keep implementation plans in the downstream
planning phase.

Present the PRD and request explicit approval of its current revision. If the
artifact changes during review, identify the new revision when requesting
approval. The approved product scope is the input to specification and
planning.
