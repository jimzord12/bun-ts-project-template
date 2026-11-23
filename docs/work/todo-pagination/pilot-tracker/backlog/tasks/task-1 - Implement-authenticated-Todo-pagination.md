---
id: TASK-1
title: Implement authenticated Todo pagination
status: Blocked
assignee: []
created_date: '2026-11-23T18:31:43Z'
updated_date: '2026-11-23T18:31:44Z'
labels: []
milestone: m-0
dependencies: []
references:
  - docs/work/todo-pagination/TODO.md#p1
  - docs/work/todo-pagination/SPEC.md#list-contract
  - docs/work/todo-pagination/PLAN.md#p1
type: feature
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
## Outcome
A signed-in client can traverse its matching todos through bounded pages.

## Scope
Add pagination to `GET /todos` through the existing session guard, route validation, and Todo query.

## Verification
Run `bun test src/todos` with the cases in `SPEC.md` and the page-boundary ordering check in `PLAN.md`.

## What unblocks this task
The API Starter supplies the runnable Todo listing, session guard, published validation response, and authenticated test fixtures with a passing baseline run of `bun test src/todos`.

## Source
- [Planning item](../../../TODO.md#p1)
- [Specification](../../../SPEC.md#list-contract)
- [Implementation plan](../../../PLAN.md#p1)
- [Starter prerequisite](../../../PLAN.md#prerequisite)
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 The route accepts the specified parameters and defaults, returns the bounded envelope, and uses the shared error response for invalid requests.
- [ ] #2 A filtered multi-page traversal stays in stable order, returns only owned matching rows, and offers continuation only when another matching row exists.
- [ ] #3 Empty, final, and beyond-end pages terminate correctly, and unauthenticated requests return `401`.
<!-- AC:END -->
