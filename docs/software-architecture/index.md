# Software Architecture

Structural decisions about a system: how it's decomposed into components,
how those components talk to each other, and where data lives. These
decisions are expensive to reverse — the pages here focus on the trade-offs
behind each choice, not just the definitions.

## Pages in this topic

| Page | Covers |
|---|---|
| [API Design](api.md) | General principles that apply across REST/GraphQL/gRPC |
| [REST](rest.md) | Resource modeling, status codes, HATEOAS |
| [GraphQL](graphql.md) | Schema design, resolvers, N+1 problem |
| [gRPC](grpc.md) | Protocol Buffers, streaming, when to prefer it over REST |
| [Microservices](microservices.md) | Service boundaries, communication, data ownership |
| [Event-Driven Architecture](event-driven.md) | Pub/sub, event sourcing, CQRS |

!!! note "Quality attributes"
    Every choice on these pages trades off against **scalability**,
    **availability**, **latency**, **consistency**, and **maintainability**.
    There's no universally "best" answer — only the right trade-off for a
    given system's actual requirements.
