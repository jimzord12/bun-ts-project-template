# Todo pagination

**Status** — Revision 1 approved by the maintainer for the design-to-task pilot

## User need

A client can list its todos and filter them by completion. Clients with
long lists need to retrieve those results in bounded pages and continue
until they reach the end of the list.

## Scope and constraints

Add pagination to `GET /todos`. Preserve the route's session requirement,
per-user ownership, completion filter, and consistent ordering. The design
applies to the reference Todo API included in the template.

## Success criteria

- A signed-in client can retrieve a bounded page and determine whether more
  results are available.
- A client using the same completion filter can traverse its matching todos
  across pages without gaps or duplicates while the underlying data is
  unchanged.
- Each page contains only todos owned by the signed-in user, in the consistent
  order defined for the listing route.
- An empty result and the end of a result set are distinguishable from a
  page with more results.

## Decisions for specification

Define the pagination parameters, continuation method, page-size defaults
and limits, return shape, and validation behavior. Cover multi-page, empty,
filtered, and cross-account cases in the verification plan.

## Sources

- [Reference Todo API and account requirements](../../template-design.md#reference-todo-api)
- [Todo pagination work example](../../work-organization.md#work-types)
