# API Gateway

## What is an API Gateway?

An **API Gateway** is the single entry point for clients to access backend services.

Instead of clients calling each microservice directly, they send requests to the gateway, which forwards them to the appropriate service.

---

# Without API Gateway

```text
                Client
              /    |     \
             /     |      \
            /      |       \
      User API  Order API  Payment API
```

Problems:

- Client must know every service.
- More network calls.
- Authentication implemented in multiple services.
- Hard to modify service addresses.
- Harder to monitor traffic.

---

# With API Gateway

```text
                Client
                   |
          +----------------+
          | API Gateway    |
          +----------------+
            /      |      \
           /       |       \
      User API  Order API  Payment API
```

The client only communicates with the gateway.

---

# Why do we need an API Gateway?

In a microservices architecture, there may be dozens or even hundreds of services.

Without a gateway:

- Clients must know every endpoint.
- Authentication is duplicated.
- Routing logic is spread across clients.
- Service changes affect clients.

The API Gateway solves these problems by acting as a single entry point.

---

# Responsibilities of an API Gateway

## 1. Request Routing

```text
GET /orders

↓

API Gateway

↓

Order Service
```

The gateway forwards requests to the correct service.

---

## 2. Authentication & Authorization

```text
Client

↓

JWT Token

↓

API Gateway

↓

Validate Token

↓

Forward Request
```

Instead of every service validating JWTs, the gateway can perform the initial authentication and authorization.

---

## 3. Rate Limiting

Prevent abuse by limiting requests.

```text
Client

↓

1000 requests/sec ❌

↓

Gateway

↓

100 requests/sec ✅
```

---

## 4. Load Balancing

The gateway can distribute requests across multiple instances of the same service.

```text
Gateway

↓

Order Service A

Order Service B

Order Service C
```

---

## 5. Response Caching

Frequently requested data can be cached.

```text
Client

↓

Gateway Cache

↓

If found → Return immediately

Else → Call backend
```

Benefits:

- Faster responses
- Reduced backend load

---

## 6. Logging & Monitoring

The gateway records:

- Request count
- Response time
- Error rate
- User activity

This helps with monitoring and troubleshooting.

---

## 7. SSL/TLS Termination

HTTPS connections can end at the gateway.

```text
HTTPS

↓

Gateway

↓

HTTP (internal network)
```

This reduces encryption overhead on backend services while keeping external communication secure.

---

# Advantages

- Single entry point for clients
- Simplifies client applications
- Centralizes authentication and authorization
- Supports rate limiting
- Supports response caching
- Provides monitoring and logging
- Enables seamless backend changes
- Hides internal services from external users
- Works with services written in different languages

---

# Considerations

## Do NOT put business logic in the gateway.

Good:

- Authentication
- Routing
- Caching
- Rate limiting

Bad:

- Calculate discounts
- Process payments
- Create orders

Business logic belongs in backend services.

---

## Single Point of Failure

One gateway instance is risky.

Production deployment:

```text
             Client
                |
        Load Balancer
           /       \
          /         \
 Gateway A       Gateway B
      |               |
   Microservices
```

Deploy multiple gateway instances behind a load balancer.

---

## Prevent Gateway Bypass

Good:

```text
Internet

↓

API Gateway

↓

Microservices
```

Bad:

```text
Internet

↓

Order Service
```

Backend services should only be accessible internally.

---

# API Gateway vs Load Balancer

| API Gateway | Load Balancer |
|--------------|---------------|
| Routes based on API paths | Distributes traffic |
| Authentication | No authentication (typically) |
| Rate limiting | No |
| Caching | Usually no |
| Logging | Basic |
| Business-aware routing | No |
| Works at application layer (L7) | Often Layer 4 or Layer 7 |

A gateway often includes load-balancing capabilities, but a load balancer is not a full API Gateway.

---

# Common API Gateway Products

- Kong
- NGINX
- Spring Cloud Gateway
- Envoy
- AWS API Gateway
- Azure API Management
- Apigee

---

# Interview Questions

## Q1. What is an API Gateway?

**Answer**

An API Gateway is the single entry point for client requests. It routes requests to backend services and handles cross-cutting concerns such as authentication, rate limiting, caching, and logging.

---

## Q2. Why do we need an API Gateway?

**Answer**

To simplify client communication, centralize common functionality, improve security, and hide the internal microservice architecture.

---

## Q3. Should an API Gateway contain business logic?

**Answer**

No.

It should only handle infrastructure concerns such as routing, authentication, caching, logging, and rate limiting. Business logic belongs in backend services.

---

## Q4. Is an API Gateway a Single Point of Failure?

**Answer**

It can be.

In production, multiple gateway instances are deployed behind a load balancer for high availability.

---

## Q5. What's the difference between an API Gateway and a Load Balancer?

**Answer**

A load balancer mainly distributes traffic across service instances.

An API Gateway provides additional features such as authentication, authorization, routing, caching, rate limiting, logging, and API aggregation.

---

## Q6. Can clients directly call microservices?

**Answer**

Generally, no.

External clients should access services only through the API Gateway to enforce security and maintain a consistent API surface.

---

## Q7. Can an API Gateway communicate with services written in different programming languages?

**Answer**

Yes.

As long as services expose standard protocols such as HTTP or gRPC, the gateway can route requests regardless of the implementation language.

# Popular API Gateway Frameworks

## 1. Spring Cloud Gateway (Java/Spring)

Most common in Spring Boot microservices.

Features:

- Request routing
- JWT Authentication
- Rate limiting
- Load balancing
- Circuit breaker (Resilience4j)
- Request/Response filters
- Path rewriting

Example:

```text
Client

↓

Spring Cloud Gateway

↓

User Service

Order Service

Payment Service
```

Example configuration:

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: order-service
          uri: http://localhost:8081
          predicates:
            - Path=/orders/**
```

Best for:

- Spring Boot
- Java microservices

---

## 2. Kong Gateway

Open-source API Gateway built on NGINX.

Features:

- Authentication
- JWT
- OAuth2
- Rate limiting
- Logging
- Caching
- Plugin ecosystem

Architecture:

```text
Client

↓

Kong Gateway

↓

Microservices
```

Best for:

- Cloud-native applications
- Kubernetes

---

## 3. NGINX

Although primarily a web server and reverse proxy, NGINX is commonly used as an API Gateway.

Features:

- Reverse proxy
- Load balancing
- SSL termination
- Caching
- URL routing

Example:

```text
Client

↓

NGINX

↓

User Service
Order Service
Payment Service
```

Example configuration:

```nginx
location /orders {
    proxy_pass http://order-service;
}
```

---

## 4. Envoy Proxy

High-performance proxy developed by Lyft.

Commonly used with Istio.

Features:

- Service discovery
- Load balancing
- Authentication
- Observability
- gRPC support
- HTTP/2 support

Architecture:

```text
Client

↓

Envoy

↓

Microservices
```

Best for:

- Kubernetes
- Service Mesh

---

## 5. AWS API Gateway

Managed API Gateway on AWS.

Features:

- No server management
- Lambda integration
- Authentication
- Rate limiting
- Monitoring
- API keys

Architecture:

```text
Client

↓

AWS API Gateway

↓

Lambda

or

EC2 Services
```

Best for:

- Serverless applications
- AWS cloud

---

## 6. Azure API Management

Microsoft's managed API Gateway.

Features:

- Authentication
- API versioning
- Monitoring
- Developer portal
- Rate limiting

Best for:

- Azure ecosystem

---

## 7. Apigee (Google Cloud)

Enterprise API management platform.

Features:

- API analytics
- OAuth2
- Developer portal
- Monetization
- Security

Best for:

- Large enterprise APIs

---

# Comparison

| Framework | Open Source | Cloud | Best For |
|------------|-------------|--------|----------|
| Spring Cloud Gateway | ✅ | ❌ | Spring Boot microservices |
| Kong | ✅ | Optional | General microservices |
| NGINX | ✅ | ❌ | Reverse proxy + API Gateway |
| Envoy | ✅ | ❌ | Kubernetes & Service Mesh |
| AWS API Gateway | ❌ | AWS | Serverless & AWS |
| Azure API Management | ❌ | Azure | Azure applications |
| Apigee | ❌ | GCP | Enterprise API management |