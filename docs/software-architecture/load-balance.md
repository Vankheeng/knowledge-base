# Load Balancing

## What is Load Balancing?

Load balancing is the process of distributing incoming client requests across multiple servers so that no single server becomes overloaded.

Without a load balancer:

```text
                1000 Requests
                     |
                  Server A
             CPU = 100% ❌
```

With a load balancer:

```text
                1000 Requests
                     |
              +----------------+
              | Load Balancer  |
              +----------------+
               /      |       \
              /       |        \
        Server A  Server B  Server C
          30%       35%       35%
```

---

# Why do we need Load Balancing?

When a system scales **horizontally**, we run multiple servers instead of one powerful server.

Instead of every client choosing a server, a **load balancer acts as the coordinator** and forwards each request to an appropriate server.

Benefits:

- Prevents one server from becoming overloaded
- Improves system performance
- Enables horizontal scaling
- Increases availability
- Supports fault tolerance
- Makes maintenance easier (servers can be updated one at a time)

---

# Horizontal Scaling

Instead of

```text
Client
  |
Server (8 CPU)
```

we scale like this:

```text
                Clients
                   |
           +----------------+
           | Load Balancer  |
           +----------------+
             /     |      \
            /      |       \
       Server1 Server2 Server3
```

---

# Types of Load Balancers

## 1. DNS Load Balancing

How it works:

```text
Client
   |
DNS Query
   |
DNS Server
   |
Returns one IP address

Example:
example.com
↓

192.168.1.10
```

Sometimes DNS rotates the returned IP:

```text
Request 1 -> Server A
Request 2 -> Server B
Request 3 -> Server C
```

This is called **Round Robin DNS**.

### Advantages

- Very simple
- Cheap
- No extra infrastructure

### Disadvantages

- Cannot accurately detect unhealthy servers
- DNS caching means users may continue using a dead server
- Cannot make intelligent routing decisions
- Cannot consider server CPU or memory usage

---

## 2. Hardware Load Balancer

A dedicated physical appliance.

Example:

```text
              Clients
                  |
         Hardware Load Balancer
                  |
      +-----------+-----------+
      |           |           |
   Server A    Server B    Server C
```

Examples:

- F5 BIG-IP
- Citrix ADC

### Advantages

- Very high performance
- Reliable
- Excellent health checking
- Advanced routing

### Disadvantages

- Expensive
- Requires dedicated hardware
- Harder to scale than software solutions

---

## 3. Software Load Balancer

Software running on a VM, server, or container.

Examples:

- Nginx
- HAProxy
- Envoy
- Traefik

Architecture:

```text
              Clients
                  |
          Nginx / HAProxy
                  |
      +-----------+-----------+
      |           |           |
   Server A    Server B    Server C
```

### Advantages

- Low cost
- Easy to configure
- Easy to scale
- Supports health checks
- Flexible

### Disadvantages

- Still consumes CPU/RAM on the machine it runs on
- Usually less powerful than high-end hardware appliances

> Note:
>
> A software load balancer still runs on a machine (physical server, VM, or container). It does **not** magically work without compute resources.

---

## 4. Global Server Load Balancer (GSLB)

Used when servers are located in multiple geographic regions.

Example:

```text
                 User

                  |
              GSLB (DNS)

      +-----------+-----------+
      |                       |
  US Datacenter          Asia Datacenter
      |                       |
   Load Balancer          Load Balancer
      |                       |
 Servers...               Servers...
```

The GSLB chooses the best region based on:

- User location
- Latency
- Server health
- Capacity
- Policies

### Advantages

- Supports worldwide users
- High availability
- Disaster recovery
- Routes users to the nearest healthy region

### Disadvantages

- More complex
- More expensive
- Usually cloud or enterprise solutions

Examples:

- AWS Route 53
- Azure Traffic Manager
- Cloudflare Load Balancing

---

# Load Balancing Algorithms

## Round Robin

```text
Req1 -> A
Req2 -> B
Req3 -> C
Req4 -> A
```

Simple but ignores server capacity.

---

## Weighted Round Robin

```text
Server A Weight = 5
Server B Weight = 2
Server C Weight = 1

Requests:

A A A A A B B C
```

Powerful servers receive more requests.

---

## Least Connections

Send new requests to the server with the fewest active connections.

Example:

```text
Server A : 100 connections

Server B : 20 connections

Server C : 15 connections

↓

Next request → Server C
```

---

## IP Hash

The client's IP determines which server is selected.

Useful when a user should consistently reach the same server.

---

# Health Check

A load balancer periodically checks whether servers are healthy.

```text
Load Balancer

↓

GET /health

↓

Server A → 200 OK ✅

Server B → Timeout ❌

↓

Do not send requests to Server B
```

---

# Interview Questions

## Q1. Why do we need a load balancer?

**Answer**

To distribute incoming requests across multiple servers, preventing overload, improving performance, increasing availability, enabling horizontal scaling, and simplifying maintenance.

---

## Q2. What is horizontal scaling?

**Answer**

Horizontal scaling means adding more servers instead of increasing the power of a single server.

---

## Q3. What's the difference between horizontal and vertical scaling?

**Answer**

Vertical scaling increases the resources (CPU, RAM) of one server.

Horizontal scaling adds more servers.

---

## Q4. Does DNS load balancing know whether a server is down?

**Answer**

Not by itself.

Traditional Round Robin DNS simply rotates IP addresses and cannot accurately detect server health. Because of DNS caching, clients may still connect to an unavailable server.

---

## Q5. Can software load balancers perform health checks?

**Answer**

Yes.

Modern software load balancers like Nginx and HAProxy can periodically check backend server health.

---

## Q6. What is the difference between a hardware and software load balancer?

| Hardware | Software |
|-----------|----------|
| Dedicated appliance | Runs on a server/VM/container |
| Expensive | Low cost |
| Very high performance | Flexible and scalable |
| Enterprise data centers | Most cloud-native applications |

---

## Q7. What is GSLB?

**Answer**

Global Server Load Balancing distributes users across multiple regions or data centers based on latency, health, location, or routing policies.

---

## Q8. Which load balancing algorithm should be used if servers have different capacities?

**Answer**

Weighted Round Robin.

More powerful servers receive a larger share of traffic.

---

## Q9. Which algorithm works best when requests have long-lived connections?

**Answer**

Least Connections.

It balances based on the number of active connections rather than simply counting requests.

---

## Q10. Is a load balancer a single point of failure?

**Answer**

It can be.

In production, organizations often deploy multiple load balancers with failover mechanisms (active-active or active-passive) to avoid a single point of failure.