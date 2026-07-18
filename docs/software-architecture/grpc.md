# gRPC

A binary RPC framework built on HTTP/2 and Protocol Buffers, designed for
fast, strongly-typed service-to-service communication.

## Protocol Buffers

Contracts are defined in `.proto` files and compiled into client/server
code for many languages — the schema is enforced at compile time, not
discovered at runtime like a JSON API:

```protobuf
service OrderService {
  rpc GetOrder (OrderRequest) returns (OrderResponse);
  rpc StreamOrders (OrderFilter) returns (stream OrderResponse);
}

message OrderRequest {
  string order_id = 1;
}
```

Binary encoding is smaller and faster to (de)serialize than JSON — a real
win at high request volume between internal services.

## Streaming

Beyond simple request/response, gRPC supports:

- **Server streaming** — one request, a stream of responses (e.g. live
  price updates).
- **Client streaming** — a stream of requests, one response (e.g.
  uploading chunks).
- **Bidirectional streaming** — both sides stream independently (e.g. a
  chat connection).

## When to prefer gRPC over REST

| Favor gRPC when | Favor REST when |
|---|---|
| Internal service-to-service calls | Public-facing APIs (browser clients, third parties) |
| Need streaming | Simple request/response is enough |
| Performance/latency is critical | Human-readable payloads matter for debugging |
| Polyglot microservices with shared `.proto` contracts | Ecosystem/tooling maturity for REST is a requirement |

!!! note
    Browsers can't call gRPC directly without a proxy (gRPC-Web) — this is
    the main reason gRPC dominates internal service meshes but REST/GraphQL
    still dominate public and browser-facing APIs.
