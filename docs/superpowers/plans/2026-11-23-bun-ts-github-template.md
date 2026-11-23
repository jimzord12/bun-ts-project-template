# Bun TypeScript GitHub Template Implementation Plan

We are defining a small, reusable GitHub template for Bun and TypeScript projects. This draft records the scope, the decisions we need to make, and the order in which to build and validate the template.

## Baseline

- Pin Bun 1.4.5 and TypeScript 7 for the first version. Check the same runtime and lockfile in local development and CI.
- Use Backlog.md for Git-tracked work. Keep the format and agent instructions useful to a maintainer working alone or with several agents.
- Include Neverthrow and a compact example that shows how to return and handle an expected failure.
- Use trunk-based development with `main` as the trunk. Start with short-lived change branches and squash PRs. Integrate small slices frequently.
- Keep the starter general-purpose so the same repository layout can support a CLI, service, or library. Decide which starter example best teaches the conventions without committing consumers to one of those shapes.

## Design sessions

| Session | Questions to settle | Output |
| --- | --- | --- |
| Work types | Which types change how work is handled? Test the first list against bugs, features, enhancements, spikes, maintenance, refactors, docs, dependencies, and urgent fixes. Which are types and which are labels or urgency? | A small work taxonomy with an intake example for each type. |
| Triage | Where do reports enter? Who decides readiness and priority? How do agents handle duplicates, missing evidence, blocked work, and discoveries outside the current task? | A triage playbook with Backlog.md status transitions and concrete examples. |
| Design and implementation | What fits directly in a task? Which changes warrant a design discussion or a decision record? How does an agent find conventions and keep a PR small? | A short implementation playbook and an agent context layout. |
| Verification and review | What runs locally and in CI? What evidence does a PR need? When does human review matter, and what happens after requested changes? | A required check, a PR template, and guidance sized to the risk of the change. |
| Publishing and rollback | What counts as a release for a library, CLI, or service? Which parts of versioning, release notes, tags, approvals, and rollback are common? | A release playbook with a shared core and guidance for each distribution target we support. |
| Template maintenance | What should every generated repo inherit? Where should template design records live? Which GitHub settings and values need setup per generated repo? | A template file list, a first-run checklist, and a way to validate updates. |

Start with work types and triage so the task model is clear before designing review and release rules. Keep the resulting docs in the template and link to them from a short `AGENTS.md`.

## Build and validate

1. **Toolchain.** Create the package manifest, lockfile, strict TypeScript configuration, and local scripts for formatting, linting, type checking, and testing. Verify that a fresh install using the lockfile can run every check with the pinned toolchain.
2. **Starter code.** Add the agreed example and a Bun test that exercises success and an expected failure. Verify the example through its documented entry point and type-check with TypeScript 7.
3. **Work playbooks.** Initialize Backlog.md and add the agreed workflow docs, agent entry point, and PR template. Walk through one bug and one spike to check that an agent can find the next step and record the result.
4. **GitHub checks and release handoff.** Run the same local checks in GitHub Actions. Document how a maintainer configures `main` protections under their GitHub plan and follows the release playbook.
5. **Template trial.** Once published as a GitHub template, generate a fresh repository using the default branch. Verify setup, task creation, a sample PR with CI, and the per-repository setup checklist. Inspect the generated files for template-specific planning artifacts and sample work before calling the template ready.

## References

- [GitHub template repositories](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository)
- [Creating a repository from a template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)
- [Trunk-based development with short-lived branches](https://trunkbaseddevelopment.com/short-lived-feature-branches/)
## Decisions from design sessions

### Work Organization

- Use four work types. A Bug requires evidence of the expected and observed behavior. A Feature requires acceptance criteria for the desired behavior. Maintenance states what needs attention and what must keep working. A Spike starts with a question and a timebox and ends with evidence and a recommendation.
- Use labels for topics such as dependencies and docs. Record urgency as priority.
- Close a change task after verification and merge. When its acceptance criteria require deployment, close it after deployment and a live check.
- Give each task one primary work type based on its intended outcome. Split out work that is worth delivering and verifying independently.
- Use Bug to restore an established expectation and Feature to introduce a new one. A confirmed Bug keeps its type even when the cause is unknown. Link a separate Spike when the investigation needs its own timebox and decision.
- Use Maintenance with a `refactor` label for a standalone refactor. Include a refactor within another task when it is part of delivering that task's outcome.

#### Work Flows

- Take in work from GitHub issues, direct requests, and discoveries during other work. Give each piece of tracked work one canonical Backlog.md task and link to the original report when a durable reference exists.
- Mark a task ready when its outcome, scope, and verification are clear, it has the evidence required for its work type, and its blockers to starting are resolved.
- Agents handle routine triage under project rules, including classification, duplicate checks, and readiness. Maintainers set priority policy and resolve product trade-offs.
- Keep reports that lack essential evidence in intake and record what is needed to proceed. Keep well-defined tasks with unresolved dependencies in the backlog, link the dependency, and record what will unblock them.
- Link duplicate reports to the canonical task and carry over new evidence.
- Record actionable discoveries outside the current scope as follow-up tasks. If a discovery prevents completion, record it as a blocker on the current task.

##### Task feedback protocol draft

1. Capture issues and friction in the current Backlog.md task's implementation notes under **Task feedback**. Record what happened, the impact, any evidence that will help someone follow up, and a proposed fix or open question.
2. During wrap-up, search the backlog for an existing task about the same issue. Add the new evidence and link the task when one exists. Open a linked improvement task when the issue merits independent work and has a clear outcome or a question worth investigating.
3. When the improvement is delivered, verify it against the original friction. Record the result on the improvement task so future agents can follow the links from the observation to the fix.

Example. During the fresh-checkout test, the agent finds that the README skips the database migration step. The note records the failed startup, the missing instruction, and the proposed README change. A linked Maintenance task adds the step and verifies startup from a fresh checkout.

### Design tree

**Root** — A GitHub template for small-to-medium JSON APIs.

1. **API Starter** — Deliver a runnable Elysia API with Zod, Neverthrow, Drizzle, SQLite, Better Auth, a tested Todo example, and a guide to removing auth.
2. **Work Organization** — Define work types, work flows, and Backlog.md conventions.
3. **Design and Implementation** — Define how changes are designed and delivered through short-lived PRs into main.
4. **Verification and Review** — Define shared local and CI checks and the evidence expected in review.
5. **Publishing and Rollback** — Define how to release, verify, and roll back a generated API.
6. **Template Setup and Maintenance** — Define first-run setup and validate template changes with a fresh generated repo.
7. **Continuous Improvement and Self-Healing** — Use task feedback to identify recurring failure modes. Decide which failures the repo can detect and recover from automatically, how it proves recovery, and when it hands off to a maintainer.
