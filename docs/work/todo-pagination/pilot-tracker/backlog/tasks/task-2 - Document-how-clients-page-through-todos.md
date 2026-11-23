---
id: TASK-2
title: Document how clients page through todos
status: Blocked
assignee: []
created_date: '2026-11-23T18:31:43Z'
updated_date: '2026-11-23T18:31:45Z'
labels: []
milestone: m-0
dependencies:
  - TASK-1
references:
  - docs/work/todo-pagination/TODO.md#p2
  - docs/work/todo-pagination/SPEC.md#list-contract
  - docs/work/todo-pagination/PLAN.md#p2
type: maintenance
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
## Outcome
A client can follow the API guide to retrieve the next page and stop at the end.

## Scope
Add or update the starter README's reference Todo API section with the verified `GET /todos` pagination contract.

## Verification
Repeat the guide's requests against the running local API with a test account and matching fixture data.

## What unblocks this task
Completion of planning item P1.

## Source
- [Planning item](../../../TODO.md#p2)
- [Specification](../../../SPEC.md#list-contract)
- [Implementation plan](../../../PLAN.md#p2)
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 The guide states the parameters, defaults, response envelope, and continuation behavior from the spec.
- [ ] #2 A filtered two-page example carries the completion filter forward, uses the returned offset, and shows how to recognize the end.
<!-- AC:END -->
