# Template design

**Root** — A GitHub template for small-to-medium JSON APIs.

Every generated project starts with the same toolchain, a replaceable reference
feature, and operating playbooks. The Todo API demonstrates how those choices
fit together through real requests, data, expected failures, and tests.

## Milestones

These are the first-level deliverables of the design.

| Milestone | Intended outcome |
| --- | --- |
| API Starter | A runnable API with the agreed stack, account management, and five tested Todo operations. |
| Work Organization | A maintainer or agent can take a report from intake through triage and into Backlog.md, then hand it off or close it. |
| Design and Implementation | Agents can tell which changes need a design discussion and deliver small changes through short-lived PRs into `main`. |
| Verification and Review | Local and CI checks agree, PRs carry verification evidence, and agents know when review needs a human. |
| Publishing and Rollback | A maintainer can release a generated API, verify the deployment, and roll it back. |
| Template Setup and Maintenance | A fresh generated repository can follow the setup guide, run the checks, create a task, and deliver a sample PR. |
| Continuous Improvement and Self-Healing | Use task feedback to find recurring failures. Decide what the repo can detect and recover from automatically, how to verify recovery, and when to hand off to a maintainer. |

## Stack and dependencies

| Role | Choice | Dependency or tool |
| --- | --- | --- |
| Runtime and package manager | Bun **1.4.5** | Bun binary, `bun.lock`, `bun install --frozen-lockfile` in CI |
| Type system | TypeScript **7**, strict checking | `typescript` and `@types/bun` as dev dependencies. Verify the exact TypeScript 7 patch and type-check entry point before scaffolding. |
| HTTP | Elysia on Bun | `elysia` |
| Validation | Zod for request bodies, path params, query strings, and environment configuration | `zod`. Use Elysia's Standard Schema integration for route schemas. |
| Expected business failures | Neverthrow | `neverthrow`. Map typed application errors to HTTP responses at the route boundary. |
| Persistence | Drizzle ORM over Bun's built-in SQLite driver | `drizzle-orm` with `drizzle-orm/bun-sqlite` and `bun:sqlite`. Use `drizzle-kit` as a dev dependency for versioned migrations. |
| Accounts and sessions | Better Auth, using the same SQLite database through Drizzle | `better-auth`, `@better-auth/drizzle-adapter`. Consider a pinned `auth` CLI as a dev tool if schema generation is part of the repository workflow. |
| Tests | Bun test runner | `bun:test`, built into Bun |
| Work tracking | Git-tracked Backlog.md | `backlog.md` CLI. Decide how to pin and invoke it in the Backlog.md conventions session. |
| Integration | Trunk-based development | Git and GitHub. Use short-lived PR branches and squash merge into `main`. |
| CI | Same checks and versions as local development | GitHub Actions. Define the required check, formatter, and linter in Verification and Review. |

Direct dependency versions are to be verified together and pinned before the
starter is built. TypeScript `7.0.2` was available in the package registry at
this checkpoint and is a candidate. The workstation reported Bun `1.4.4`
while the intended template baseline is `1.4.5`.

Official docs for the dependency choices include [Bun types and TypeScript](https://bun.com/docs/typescript),
[Elysia validation](https://elysiajs.com/essential/validation),
[Drizzle and Bun SQLite](https://orm.drizzle.team/docs/get-started/bun-sqlite-new),
[Better Auth's Drizzle adapter](https://better-auth.com/docs/adapters/drizzle),
[Better Auth tooling](https://better-auth.com/docs/concepts/cli), and
[Backlog.md](https://github.com/MrLesk/Backlog.md).

## Reference Todo API

Keep the Todo routes, use cases, and their tests in one replaceable feature
area such as `src/todos/`. New projects replace the Todo feature and its
documentation examples. The reusable starting point includes server wiring,
validation and error conventions, the shared database and migration setup,
auth integration, test conventions, checks, and playbooks.

| Route | Pattern to teach |
| --- | --- |
| `POST /todos` | Create a todo and return `201`. Validate the request shape and show a typed error for an expected invalid title. |
| `GET /todos` | Validate and apply a completion filter, then return a consistently ordered list. |
| `GET /todos/:id` | Return the owned todo or map a missing record to `404`. |
| `PATCH /todos/:id` | Validate and persist a partial update. A typed missing-todo error maps to `404`. |
| `DELETE /todos/:id` | Delete the owned todo, return `204`, and verify the row is gone. |

For example, Zod checks the shape of a PATCH request, including the boolean
value of `completed` when supplied. The use case uses Neverthrow for a missing
todo. The route translates the failure into an HTTP response.

## Accounts and route protection

- Use Better Auth's self-service operations for registration, sign-in, sign-out,
  account updates, and account deletion. Registration takes a display name,
  email, and password. Use cookie-based sessions.
- Expose `GET /me` for the signed-in profile. Support display name changes and
  password changes. Email changes use verification when an email sender is
  configured.
- Require identity confirmation for account deletion. Verify that deletion
  invalidates the account's sessions and removes its sample todos.
- Provide a reusable Elysia route guard. It resolves the session, returns
  `401` when authentication is required, and supplies the user ID to protected
  handlers. Scope Todo queries to that ID. A second user's attempt to change
  a todo should return `404` and leave the row intact.
- Document how to remove authentication from a generated repo, including auth
  wiring, the route guard, environment configuration, packages, tests, and
  Todo ownership. Cover how a project with existing data would migrate the
  schema.

[Better Auth's Elysia integration](https://better-auth.com/docs/integrations/elysia)
and [account operations](https://better-auth.com/docs/concepts/users-accounts)
are the references for this design.

## Current design boundary

[Work Organization](work-organization.md) records the decisions reached so
far in that milestone. Use the [handoff](HANDOFF.md) to resume the design
discussion before turning the design into an implementation plan.
