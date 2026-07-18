# API Design Principles

Principles that hold regardless of which protocol (REST/GraphQL/gRPC) sits
on top.

## Design for the consumer, not the database

An API's shape should reflect how clients need to use the data, not how
it's stored internally. Leaking internal schema details into the API
contract makes both sides harder to evolve independently.

## Versioning

- **URI versioning** (`/v1/orders`) — simple, visible, but can lead to
  duplicated logic across versions.
- **Header versioning** (`Accept: application/vnd.api.v2+json`) — keeps URLs
  clean, less discoverable.
- Prefer **additive, backward-compatible changes** (new optional fields)
  over bumping a version at all, when possible.

## Idempotency

Any operation that might be retried (network blip, client timeout) should
be safe to run twice. `PUT` and `DELETE` are idempotent by definition;
`POST` typically isn't — use an **idempotency key** header for POST
endpoints that create resources, so a retried request doesn't create a
duplicate.

## Pagination

!!! tip "Prefer cursor-based over offset-based at scale"
    `OFFSET 100000` forces the database to scan and discard 100,000 rows.
    A cursor (`?after=<opaque-id>`) lets the database jump straight to the
    right position and stays correct even if rows are inserted/deleted
    mid-pagination.

## Error responses

Return structured, machine-parseable errors — a stable `code`, a
human-readable `message`, and enough context to act on it:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "email must be a valid address",
    "field": "email"
  }
}
```
