# CAP Theorem

## What is the CAP Theorem?

The **CAP Theorem**, proposed by Eric Brewer, states:

> A distributed database **cannot simultaneously guarantee Consistency, Availability, and Partition Tolerance when a network partition occurs.**

When the network is healthy, a system may provide all three. The trade-off happens **during a network partition**.

---

# What is a Distributed System?

Instead of one database server:

```text
Database
```

We have multiple database servers.

```text
        Client
           |
    -----------------
    |               |
 Database A     Database B
```

These servers communicate over a network.

---

# The Three Properties

## 1. Consistency (C)

Every client sees the **same data** after a successful write.

Example:

```text
Client 1

↓

Write Balance = $900

↓

Database A

↓

Database B

↓

Client 2 reads

↓

Balance = $900 ✅
```

Everyone sees the latest committed data.

---

## 2. Availability (A)

Every request receives a response.

Even if some servers fail, the system still responds.

Example:

```text
Database A ❌

↓

Database B

↓

Client

↓

Response ✅
```

The response may not always contain the latest data.

---

## 3. Partition Tolerance (P)

The system continues operating even if communication between servers is interrupted.

Example:

```text
Database A

XXXXXXXX Network Failure XXXXXXXX

Database B
```

The databases cannot communicate, but the system continues to function.

---

# What is a Network Partition?

A network partition occurs when servers cannot communicate because of network failures.

```text
Client

      |

Database A

XXXXXXXXXX

Database B
```

The databases are alive but disconnected.

---

# Why Can't We Have All Three?

Suppose:

```text
Database A

XXXXXXXXXX

Database B
```

A client updates Database A:

```text
Balance = $900
```

Another client reads from Database B.

Database B still has:

```text
Balance = $1000
```

You now have two choices.

---

## Choice 1 — Consistency (CP)

Database B refuses to answer until it can synchronize.

```text
Client

↓

Read Request

↓

Database B

↓

"Cannot respond"
```

Result:

- ✅ Consistency
- ✅ Partition Tolerance
- ❌ Availability

---

## Choice 2 — Availability (AP)

Database B immediately responds.

```text
Client

↓

Read Request

↓

Database B

↓

Balance = $1000
```

The response is available but stale.

Result:

- ✅ Availability
- ✅ Partition Tolerance
- ❌ Strong Consistency

---

# CP Systems

Prioritize:

- Consistency
- Partition Tolerance

May reject requests during network failures.

Examples:

- HBase
- ZooKeeper
- etcd

Best for:

- Banking
- Payment systems
- Inventory management

---

# AP Systems

Prioritize:

- Availability
- Partition Tolerance

May temporarily return stale data.

Examples:

- Cassandra
- Riak
- DynamoDB (configurable)

Best for:

- Social media
- Chat applications
- Product catalogs
- News feeds

---

# CA Systems

CA means:

- Consistency
- Availability

Without partition tolerance.

Example:

```text
Client

↓

Single Database Server
```

Traditional relational databases (like MySQL or PostgreSQL on a single server) behave like CA because there is no distributed network between database nodes.

However, in real distributed environments, network partitions are always possible, so pure CA systems are generally not practical.

---

# CAP Summary

| Property | Meaning |
|----------|---------|
| Consistency | Every client sees the latest data. |
| Availability | Every request receives a response. |
| Partition Tolerance | The system continues operating despite network failures. |

---

# CP vs AP

| CP | AP |
|----|----|
| Always returns consistent data | Always returns a response |
| May reject requests | May return stale data |
| Better for financial systems | Better for high-traffic web systems |

---

# Real-World Examples

## Banking (CP)

```text
Transfer $100

↓

Must update every database correctly

↓

If synchronization fails

↓

Reject transaction
```

Correctness is more important than availability.

---

## Social Media (AP)

```text
User changes profile picture

↓

Some users see old picture

↓

Others see new picture

↓

Eventually everyone sees new picture
```

Temporary inconsistency is acceptable.

---

# Popular Databases

| Database | CAP Preference |
|----------|----------------|
| MySQL (single server) | CA |
| PostgreSQL (single server) | CA |
| MongoDB | CP (default replica set behavior) |
| Cassandra | AP |
| Riak | AP |
| HBase | CP |
| ZooKeeper | CP |
| etcd | CP |

---

# Interview Questions

## Q1. What is the CAP Theorem?

**Answer**

The CAP Theorem states that during a network partition, a distributed system can guarantee at most two of the following three properties: Consistency, Availability, and Partition Tolerance.

---

## Q2. What is Consistency?

**Answer**

Every client sees the same, most recent committed data.

---

## Q3. What is Availability?

**Answer**

Every request receives a response, even if some servers have failed.

---

## Q4. What is Partition Tolerance?

**Answer**

The system continues operating even when communication between distributed nodes is interrupted.

---

## Q5. Why is Partition Tolerance important?

**Answer**

Because network failures are unavoidable in distributed systems. A distributed database must continue operating despite communication problems.

---

## Q6. What is the difference between CP and AP?

**Answer**

- **CP** prioritizes consistency over availability during a partition.
- **AP** prioritizes availability over strong consistency during a partition.

---

## Q7. Which applications should choose CP?

**Answer**

Applications where correctness is critical, such as banking, payment processing, and inventory systems.

---

## Q8. Which applications should choose AP?

**Answer**

Applications where high availability is more important than immediate consistency, such as social media, chat systems, and news feeds.