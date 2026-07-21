# Techniques to Improve Database Performance, Availability & Scalability

Modern systems often use multiple techniques together:

- Database Indexing
- Database Replication
- Database Partitioning
- Database Sharding

---

# 1. Database Indexing

## What is an Index?

An index is a data structure that allows the database to locate rows quickly without scanning the entire table.

Think of it like the index of a book.

Without an index:

```text
Book

Page 1
Page 2
Page 3
...
Page 500
```

To find "Database", you may need to scan many pages.

With an index:

```text
Index

Database → Page 325
Networking → Page 180
Spring → Page 95
```

Go directly to the page.

---

## Database Example

Without index:

```sql
SELECT * FROM users
WHERE email='alice@gmail.com';
```

Database scans every row.

```text
Row1

Row2

Row3

...

Row1000000
```

---

With index:

```text
Email Index

↓

alice@gmail.com

↓

Row 589321
```

The lookup is much faster.

---

## Common Index Types

- B+ Tree (most common)
- Hash Index
- Full-text Index
- Bitmap Index (specialized)

---

## Advantages

- Faster queries
- Faster searching
- Better sorting
- Better JOIN performance

---

## Disadvantages

- Uses additional storage
- Slows INSERT/UPDATE/DELETE because indexes must also be updated

---

# 2. Database Replication

## What is Replication?

Replication copies the **same data** from one database (Primary) to one or more replicas.

```text
          Primary Database
                 |
      ---------------------
      |                   |
 Replica 1           Replica 2
```

All replicas contain the same data.

---

## Why use Replication?

### High Availability

If the primary fails:

```text
Primary ❌

↓

Replica promoted

↓

System continues working
```

---

### Read Scalability

Write operations go to the primary.

Read operations can go to replicas.

```text
          Client

        Read Request

              ↓

 Replica A

 Replica B

 Replica C
```

This reduces load on the primary.

---

## Replication Types

### Synchronous Replication

The primary waits until replicas confirm the write.

Advantages:

- Strong consistency

Disadvantages:

- Higher write latency

---

### Asynchronous Replication

The primary commits immediately.

Replicas update later.

Advantages:

- Faster writes

Disadvantages:

- Replication lag (temporary inconsistency)

---

# 3. Database Partitioning

Partitioning splits **one large table** into smaller logical partitions **within the same database**.

Example:

Orders table

```text
Orders

↓

2023 Partition

2024 Partition

2025 Partition
```

Queries only search the relevant partition.

Example:

```sql
SELECT *

FROM Orders

WHERE order_year = 2025;
```

Only the 2025 partition is scanned.

---

## Types of Partitioning

- Range Partition
- List Partition
- Hash Partition

---

## Advantages

- Faster queries
- Easier maintenance
- Smaller indexes

---

# 4. Database Sharding

## What is Sharding?

Sharding splits **different data** across multiple database servers.

Each shard owns a subset of the data.

Example:

```text
Shard 1

Users 1 - 1,000,000

-----------------------

Shard 2

Users 1,000,001 - 2,000,000

-----------------------

Shard 3

Users 2,000,001 - 3,000,000
```

Unlike replication, shards do **not** contain the same data.

---

## Request Flow

```text
Client

↓

Shard Router

↓

Shard 2

↓

User 1,500,000
```

---

## Advantages

- Massive horizontal scalability
- Smaller databases
- Higher write throughput

---

## Challenges

- Cross-shard JOINs are difficult
- Rebalancing shards can be complex
- More application logic

---

# Replication vs Sharding

| Replication | Sharding |
|--------------|----------|
| Copies the same data | Splits different data |
| Improves availability | Improves scalability |
| Read scaling | Read and write scaling |
| Multiple copies | Each shard owns unique data |

---

# Combining Both

Large systems often use both.

```text
                  Client
                     |
                Shard Router
              /             \
             /               \
        Shard A          Shard B
          |                 |
      ---------         ---------
      |       |         |       |
 Primary Replica     Primary Replica
```

Each shard stores different data, and each shard has its own replicas for high availability.

---

# Interview Questions

## Q1. What is a database index?

**Answer**

A database index is a data structure that speeds up data retrieval by allowing the database to locate rows without scanning the entire table.

---

## Q2. What are the disadvantages of indexes?

**Answer**

Indexes require extra storage and increase the cost of INSERT, UPDATE, and DELETE operations because they must also be maintained.

---

## Q3. What is database replication?

**Answer**

Replication copies the same data from a primary database to one or more replicas to improve availability, fault tolerance, and read scalability.

---

## Q4. What is the difference between synchronous and asynchronous replication?

**Answer**

- Synchronous replication waits for replicas before committing.
- Asynchronous replication commits immediately and updates replicas later.

---

## Q5. What is database partitioning?

**Answer**

Partitioning divides a large table into smaller logical partitions within the same database to improve query performance and maintenance.

---

## Q6. What is database sharding?

**Answer**

Sharding distributes different portions of data across multiple database servers, allowing the system to scale horizontally.

---

## Q7. What is the difference between partitioning and sharding?

**Answer**

Partitioning divides a table within a single database instance, while sharding distributes data across multiple independent database servers.

---

## Q8. What is the difference between replication and sharding?

**Answer**

Replication duplicates the same data to improve availability and read performance. Sharding splits different data across servers to improve storage capacity and write scalability.