# Todo pagination task breakdown

[Approved PRD](PRD.md) · [Specification](SPEC.md) · [Implementation plan](PLAN.md)

## P1

**Title** — Implement authenticated Todo pagination
**Type** — Feature
**Outcome** — A signed-in client can traverse its matching todos through bounded pages.
**Scope** — Add pagination to `GET /todos` through the existing session guard, route validation, and Todo query.
**Depends on** — None
**Prerequisite** — The API Starter supplies the runnable Todo listing, session guard, published validation response, and authenticated test fixtures with a passing baseline run of `bun test src/todos`.
**Verification** — Run `bun test src/todos` with the cases in `SPEC.md` and the page-boundary ordering check in `PLAN.md`.

### Acceptance criteria

- The route accepts the specified parameters and defaults, returns the bounded envelope, and uses the shared error response for invalid requests.
- A filtered multi-page traversal stays in stable order, returns only owned matching rows, and offers continuation only when another matching row exists.
- Empty, final, and beyond-end pages terminate correctly, and unauthenticated requests return `401`.

## P2

**Title** — Document how clients page through todos
**Type** — Maintenance
**Outcome** — A client can follow the API guide to retrieve the next page and stop at the end.
**Scope** — Add or update the starter README's reference Todo API section with the verified `GET /todos` pagination contract.
**Depends on** — P1
**Verification** — Repeat the guide's requests against the running local API with a test account and matching fixture data.

### Acceptance criteria

- The guide states the parameters, defaults, response envelope, and continuation behavior from the spec.
- A filtered two-page example carries the completion filter forward, uses the returned offset, and shows how to recognize the end.
