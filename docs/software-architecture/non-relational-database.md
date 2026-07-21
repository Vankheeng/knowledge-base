# Non-Relational Database (NoSQL)

## What is a NoSQL Database?

A **NoSQL (Not Only SQL)** database stores data using **non-relational data models** instead of tables with fixed schemas.

Unlike relational databases, NoSQL databases are designed for:

- Flexible schemas
- Horizontal scalability
- High availability
- High read/write throughput
- Large volumes of semi-structured or unstructured data

Popular NoSQL databases:

- MongoDB
- Redis
- Cassandra
- DynamoDB
- Neo4j

---

# SQL vs NoSQL

## Relational Database (SQL)

Data is stored in tables with relationships.

```text
Users
+----+-------+
| id | name  |
+----+-------+

Orders
+---------+---------+
| orderId | userId  |
+---------+---------+

JOIN
```

Characteristics:

- Fixed schema
- Foreign keys
- JOIN operations
- ACID transactions

---

## NoSQL Database

Data is stored using different models.

```text
Key-Value
Document
Graph
```

Characteristics:

- Flexible schema
- Easy horizontal scaling
- Optimized for specific use cases
- Usually avoids expensive JOIN operations

---

# When to use NoSQL

Use NoSQL when:

- Data structure changes frequently.
- The application needs to scale horizontally.
- High read/write performance is required.
- The application stores JSON-like or hierarchical data.
- Relationships between data are simple or can be embedded.

Examples:

- Social media
- Chat applications
- Product catalogs
- IoT
- Logging systems
- Recommendation systems

---

# Types of NoSQL Databases

## 1. Key-Value Database

Stores data as a key and its associated value.

```text
"user:1"

↓

{
   "name":"Alice",
   "age":22
}
```

Similar to:

```java
Map<String, Object>
```

Examples:

- Redis
- Amazon DynamoDB
- Riak

### Advantages

- Extremely fast
- Simple data model
- Excellent for caching

### Use Cases

- Cache
- Session storage
- Shopping carts
- Authentication tokens

---

## 2. Document Database

Stores **collections of documents**.

Each document is a self-contained object (usually JSON/BSON) representing one entity.

Example:

```json
{
  "_id": 1,
  "name": "Alice",
  "age": 22,
  "address": {
    "city": "Hanoi"
  },
  "orders": [
    {
      "id": 101,
      "price": 100
    }
  ]
}
```

### Characteristics

- Stores collections of documents.
- Each document is an object.
- Different documents can have different attributes.
- Attributes can have different data types.
- Supports nested objects and arrays.
- Documents map naturally to programming language objects.

Example:

Java Object

```java
class User {
    String name;
    int age;
    Address address;
    List<Order> orders;
}
```

↓

MongoDB Document

```json
{
  "name": "Alice",
  "age": 22,
  "address": { ... },
  "orders": [ ... ]
}
```

Examples:

- MongoDB
- CouchDB
- Firebase Firestore

### Advantages

- Flexible schema
- Easy object mapping
- Great for APIs returning JSON
- Supports nested data naturally

### Use Cases

- User profiles
- Product catalogs
- CMS
- Mobile applications

---

## 3. Graph Database

Optimized for navigating and analyzing relationships between records.

Stores:

- Nodes (entities)
- Relationships (edges)
- Properties (attributes)

Example:

```text
(Alice)

   │ FRIEND

   ▼

(Bob)

   │ WORKS_WITH

   ▼

(Charlie)
```

Instead of using JOINs, graph databases efficiently traverse relationships.

Examples:

- Neo4j
- Amazon Neptune
- TigerGraph

### Advantages

- Extremely fast relationship queries
- Natural representation of connected data
- Flexible relationship modeling

### Use Cases

- Social networks
- Recommendation systems
- Fraud detection
- Knowledge graphs
- Network topology

---

# SQL vs NoSQL Comparison

| SQL | NoSQL |
|------|--------|
| Tables | Key-Value, Document, Wide-Column, Graph |
| Fixed schema | Flexible schema |
| Supports JOIN | Usually avoids JOIN |
| Strong ACID support | Varies by database |
| Vertical scaling | Horizontal scaling |
| Best for transactional systems | Best for scalable, flexible systems |

---

# Advantages of NoSQL

- Flexible schema
- Easy horizontal scaling
- High performance
- Handles large datasets
- High availability
- Optimized for specific workloads

---

# Disadvantages

- Limited JOIN support
- Data duplication may occur
- Consistency model depends on the database
- Query languages are not standardized

---

# Popular NoSQL Databases

| Type | Examples |
|------|----------|
| Key-Value | Redis, DynamoDB |
| Document | MongoDB, CouchDB, Firestore |
| Wide-Column | Cassandra, HBase, ScyllaDB |
| Graph | Neo4j, Amazon Neptune, TigerGraph |

---

# Interview Questions

## Q1. What is a NoSQL database?

**Answer**

A NoSQL database stores data using non-relational models such as key-value pairs, documents, wide columns, or graphs. It is designed for flexibility, scalability, and high performance.

---

## Q2. What are the main types of NoSQL databases?

**Answer**

1. Key-Value
2. Document
3. Graph

---

## Q3. Why is MongoDB called a document database?

**Answer**

Because it stores data as JSON/BSON documents grouped into collections. Each document is a self-contained object with a flexible schema and can contain nested objects and arrays.

---

## Q4. Why use Redis?

**Answer**

Redis is an in-memory key-value database optimized for extremely fast data access. It is commonly used for caching, session storage, and rate limiting.

---

## Q5. Why use a graph database?

**Answer**

Graph databases are optimized for querying relationships between entities, making them ideal for social networks, recommendation systems, fraud detection, and knowledge graphs.

---

## Q6. When should you choose NoSQL instead of SQL?

**Answer**

Choose NoSQL when your application requires flexible schemas, horizontal scalability, high throughput, or stores large amounts of semi-structured or connected data.