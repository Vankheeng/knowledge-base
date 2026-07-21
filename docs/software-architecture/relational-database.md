# Relational Database & ACID Transactions

## What is a Relational Database?

A **Relational Database (RDBMS)** stores data in **tables (relations)**.

Tables are connected using relationships such as:

- Primary Key (PK)
- Foreign Key (FK)

Examples:

- MySQL
- PostgreSQL
- SQL Server
- Oracle

---

# Example

```text
Users
+---------+--------+
| user_id | name   |
+---------+--------+
|    1    | Alice  |
|    2    | Bob    |
+---------+--------+

Orders
+----------+---------+--------+
| order_id | user_id | total  |
+----------+---------+--------+
|   101    |    1    | 100$   |
|   102    |    2    |  50$   |
+----------+---------+--------+
```

Relationship:

```text
Users.user_id
      │
      │
      ▼
Orders.user_id
```

---

# Why use a Relational Database?

Relational databases are best when:

- Data has relationships.
- Transactions must be reliable.
- Data integrity is important.
- Complex SQL queries and JOINs are required.
- Strong consistency is required.

Examples:

- Banking
- E-commerce
- Hospital systems
- Student management systems

---

# What is a Transaction?

A transaction is a group of operations treated as **one unit of work**.

Example:

Bank transfer

```text
Transfer $100

↓

Subtract $100 from Account A

↓

Add $100 to Account B
```

Both operations must succeed together.

---

# ACID Properties

## A — Atomicity

**Definition**

A transaction is **all or nothing**.

If one operation fails, the entire transaction is rolled back.

Example:

```text
Transfer $100

Account A -100 ✅

Account B +100 ❌

↓

Rollback

Account A restored
Account B unchanged
```

No partial updates are allowed.

---

## C — Consistency

**Definition**

A transaction moves the database from one **valid state** to another while respecting all rules and constraints.

Examples of rules:

- Primary key uniqueness
- Foreign key constraints
- CHECK constraints
- NOT NULL constraints
- Business rules

Example:

```text
Orders.user_id = 999

↓

User 999 does not exist

↓

Transaction rejected ❌
```

The database remains consistent.

---

## I — Isolation

**Definition**

Multiple transactions can run at the same time, but they should not interfere with each other in a way that produces incorrect results.

Example:

Two ATMs withdraw money simultaneously.

Without isolation:

```text
Balance = $100

ATM A reads $100

ATM B reads $100

ATM A withdraws $80

ATM B withdraws $80

Final balance = -$60 ❌
```

With proper isolation:

```text
Balance = $100

ATM A withdraws $80

Balance = $20

ATM B checks balance

Insufficient funds

Transaction rejected
```

The database prevents incorrect concurrent behavior.

---

## D — Durability

**Definition**

Once a transaction is committed, its changes are permanent.

Even if the database crashes immediately afterward, the committed data is not lost.

Example:

```text
Transfer committed ✅

↓

Power failure ⚡

↓

Restart database

↓

Money transfer still exists ✅
```

This is typically achieved using transaction logs (e.g., Write-Ahead Logging).

---

# ACID Summary

| Property | Meaning |
|-----------|---------|
| Atomicity | All operations succeed or all are rolled back. |
| Consistency | Data always satisfies rules and constraints. |
| Isolation | Concurrent transactions do not interfere incorrectly. |
| Durability | Committed changes survive crashes. |

---

# Real Banking Example

```text
Account A = $1000

Account B = $500

Transfer $200

↓

BEGIN

↓

A = 800

↓

B = 700

↓

COMMIT
```

If any step fails before `COMMIT`:

```text
ROLLBACK

A = 1000

B = 500
```

---

# Why ACID is Important

Without ACID:

- Money could disappear.
- Inventory counts could become incorrect.
- Duplicate orders could occur.
- Data corruption could happen.

---

# Popular Relational Databases

- MySQL
- PostgreSQL
- Microsoft SQL Server
- Oracle Database
- MariaDB

---

# Interview Questions

## Q1. What is a Relational Database?

**Answer**

A relational database stores structured data in tables and uses relationships (primary keys and foreign keys) to connect data. It supports SQL and ACID transactions.

---

## Q2. Why use a Relational Database?

**Answer**

Because it provides strong consistency, supports complex queries and JOINs, maintains relationships between data, and guarantees reliable transactions.

---

## Q3. What is a transaction?

**Answer**

A transaction is a sequence of database operations treated as a single unit of work. It either completes successfully or is rolled back.

---

## Q4. What is Atomicity?

**Answer**

All operations in a transaction succeed together or none of them are applied.

---

## Q5. What is Consistency?

**Answer**

Every transaction must leave the database in a valid state by satisfying all constraints and business rules.

---

## Q6. What is Isolation?

**Answer**

Transactions can execute concurrently, but each transaction should behave as if it were running independently, according to the configured isolation level.

---

## Q7. What is Durability?

**Answer**

Once a transaction is committed, the data is permanently stored and survives crashes or power failures.

---

## Q8. Why is ACID important for banking?

**Answer**

Bank transfers involve multiple updates. ACID guarantees that either all updates succeed together or none of them are applied, preventing lost money or inconsistent account balances.