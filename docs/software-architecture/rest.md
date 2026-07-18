# REST

Representational State Transfer — resources identified by URLs, manipulated
through a fixed set of HTTP verbs.

## Resource modeling

- Nouns, not verbs: `/orders/42`, not `/getOrder?id=42`.
- Nest to express ownership: `/users/7/orders` for a user's orders.
- Avoid deep nesting beyond 2 levels — prefer top-level resources with
  filter query params: `/orders?userId=7` over `/users/7/orders/items/3/tags`.

## HTTP verbs and status codes

| Verb | Meaning | Idempotent? |
|---|---|---|
| `GET` | Read | Yes |
| `POST` | Create | No |
| `PUT` | Replace | Yes |
| `PATCH` | Partial update | No (usually) |
| `DELETE` | Remove | Yes |

| Status | Meaning |
|---|---|
| `200` | OK |
| `201` | Created |
| `204` | No Content (success, empty body) |
| `400` | Bad request (client error, malformed input) |
| `401` | Unauthenticated |
| `403` | Authenticated but not authorized |
| `404` | Resource doesn't exist |
| `409` | Conflict (e.g. duplicate, version mismatch) |
| `429` | Rate limited |
| `500` | Server error |

## HATEOAS

Responses can include links to related actions/resources, so clients
discover what's possible next instead of hardcoding URL construction:

```json
{
  "id": 42,
  "status": "pending",
  "links": {
    "self": "/orders/42",
    "cancel": "/orders/42/cancel"
  }
}
```

In practice, most REST APIs skip full HATEOAS — it adds payload size and
client complexity that's rarely worth it outside of specific hypermedia use
cases.

!!! warning "Common mistake"
    Using `200 OK` for everything and putting the real status in the JSON
    body (`{"success": false}`). This breaks HTTP caching, monitoring, and
    every standard HTTP client's error handling — use real status codes.
