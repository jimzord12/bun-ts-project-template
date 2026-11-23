# Todo pagination implementation plan

Source spec — [Todo pagination specification](SPEC.md)

## Prerequisite

The [API Starter](../../template-design.md#milestones) supplies a runnable
`GET /todos` route, the session guard, the shared request-validation response,
and authenticated Todo test fixtures. These are the starting conditions for
the first implementation item. Record the shared validation response and a
passing baseline run of `bun test src/todos` as evidence that P1 can start.

## P1

**Deliver a paginated Todo list**

Add the pagination parameters and response envelope to the existing route.
Use the current guard and validation conventions. Query the owned, filtered
rows in a stable order with one extra row for continuation detection. Keep
the route, query, and integration tests in the Todo feature area.

Exercise the real HTTP query strings and response shapes with `bun test
src/todos`. In the lookahead fixture, position excluded rows after a full
page to show that another account and the opposite completion status cannot
produce a continuation offset. If the existing order uses a non-unique sort
key, put tied rows on either side of a page boundary and assert their ID
order.

## P2

**Document client traversal**

After P1 is verified, add or update the reference Todo API section of the
starter's README. Cover the parameters, defaults, response envelope, and a
two-page example using the same completion filter. Check the examples
against the running local API with a test account.

## Task handoff

Match each Backlog.md record by package path and the stable item ID in
[TODO.md](TODO.md), such as `docs/work/todo-pagination/TODO.md#p1`. Link the
applicable sections of this plan and the spec. P1 has no in-package task
dependency and is blocked by the [starter prerequisite](#prerequisite). P2
is blocked by P1 and links to its canonical task ID. Verify the final task
map against the plan before concluding the pilot.
