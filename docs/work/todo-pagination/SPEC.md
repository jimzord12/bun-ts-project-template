# Todo pagination specification

[Approved PRD](PRD.md) · [Reference API design](../../template-design.md#reference-todo-api)

## List contract

`GET /todos` returns a bounded page of the signed-in user's todos. The
existing session guard handles unauthenticated requests with `401`.
The existing `completed` filter applies to every page.

| Query parameter | Accepted values | Default |
| --- | --- | --- |
| `limit` | Decimal integer from 1 through 100 | `25` |
| `offset` | Decimal integer from 0 through JavaScript's maximum safe integer | `0` |

Validate pagination parameters with the route's Zod schema. Each decoded
value must contain only ASCII digits, with leading zeros allowed, and fit
its range. Reject empty values and repeated parameters, including repeats
with identical values. Apply defaults only when a parameter is absent.
Use the starter's shared request-validation error response. Its status and
body format must be established by the starter before route implementation.

Successful responses return `200` with `items`, an array of existing Todo
representations, and `nextOffset`, a number or `null`. Apply the owner and
completion constraints before ordering and paging. Preserve the listing's
established order and use the Todo ID as the final tie-breaker. If the starter
has yet to establish an order, use ascending ID order.

Fetch up to `limit + 1` rows at the requested offset. Return at most `limit`
items. If there is another matching row, return `nextOffset = offset +
items.length`. At the end return `nextOffset: null`. A client requests the
next page with that offset and the same completion filter. An empty match
or an offset beyond the matching results both return
`{"items":[],"nextOffset":null}`.

Offset pagination meets the approved traversal requirement for an unchanged
result set and uses the starter's existing Drizzle and SQLite stack.

## Verification

Extend the authenticated Todo route tests and run `bun test src/todos` once
the starter is runnable. Cover these cases:

- The defaults and maximum page size bound the list.
- A filtered multi-page traversal returns each matching Todo once in order.
  Include another account's matching rows in the fixture.
- A partial final page, an exact-size final page, an empty match, and an
  offset beyond the end all return `nextOffset: null`.
- On an exact-size filtered final page, add rows that sort later but belong
  to another account or fail the completion filter. The response still has
  `nextOffset: null`.
- Exercise numeric boundaries and raw query strings with empty, fractional,
  signed, whitespace, hexadecimal, exponent, and repeated values. Assert
  the status and body of the shared validation error response. Verify that
  an unauthenticated list request returns `401`.
