# Unstructured Data Storage

## What is Unstructured Data?

Unstructured data is data that does **not** fit well into rows and columns.

Examples:

- Images
- Videos
- Audio files
- PDF documents
- Office files
- Backups
- Machine learning models

Unlike relational data, it does not have a fixed schema.

---

# Structured vs Semi-Structured vs Unstructured

## Structured

```text
Users

+----+-------+------+
| ID | Name  | Age  |
+----+-------+------+
| 1  | Alice | 22   |
+----+-------+------+
```

Example:

- MySQL
- PostgreSQL

---

## Semi-Structured

```json
{
  "name": "Alice",
  "age": 22,
  "skills": [
    "Java",
    "Spring"
  ]
}
```

Example:

- MongoDB
- CouchDB

---

## Unstructured

```text
photo.jpg

video.mp4

report.pdf

music.mp3
```

Usually stored as binary files.

---

# What is a BLOB?

BLOB stands for **Binary Large Object**.

A BLOB is binary data stored as a single object.

Examples:

- Images
- Videos
- Audio
- PDFs
- ZIP files

---

# Ways to Store Unstructured Data

## 1. Store BLOBs Inside a Database

```text
MySQL

Users

----------------------

ID

ProfilePhoto (BLOB)

Resume (BLOB)
```

Advantages:

- Easy backup with database
- ACID transactions

Disadvantages:

- Database becomes very large
- Poor performance for large files
- Expensive storage

Suitable for:

- Small images
- Small documents

---

## 2. Object Storage (Recommended)

Store files separately.

Database stores only metadata.

```text
          Client

              |

Upload Image

              |

      Object Storage

              |

image123.jpg

              |

Database

↓

image_url
```

Database:

```text
Users

ID

Name

Image URL
```

Object Storage:

```text
image123.jpg

resume.pdf

video.mp4
```

Examples:

- Amazon S3
- Azure Blob Storage
- Google Cloud Storage
- MinIO

Advantages:

- Massive scalability
- Low cost
- High durability
- CDN integration
- Optimized for large files

Suitable for:

- Images
- Videos
- Backups
- Documents

---

## 3. File Storage

Traditional file systems.

```text
Server

/images

/avatar.png

/report.pdf
```

Advantages:

- Simple

Disadvantages:

- Difficult to scale
- Hard to share across multiple servers

Suitable for:

- Small applications
- Local development

---

# Typical Architecture

```text
              Client

                 |

Upload Image

                 |

Spring Boot API

        |                 |

        |                 |

Metadata           Image File

        |                 |

MySQL          Amazon S3

        |

Image URL
```

The database stores only metadata.

The actual file is stored in object storage.

---

# Comparison

| Solution | Best For | Advantages | Disadvantages |
|----------|----------|------------|---------------|
| Database BLOB | Small files | ACID, simple | Database grows quickly |
| File Storage | Local apps | Easy | Poor scalability |
| Object Storage | Large files | Cheap, scalable, durable | Requires separate storage service |

---

# Popular Solutions

## Object Storage

- Amazon S3
- Azure Blob Storage
- Google Cloud Storage
- MinIO

---

## Document Storage (Semi-Structured)

- MongoDB
- CouchDB

---

## Relational Databases

- MySQL
- PostgreSQL

---

# Interview Questions

## Q1. What is unstructured data?

**Answer**

Unstructured data is data without a predefined tabular schema, such as images, videos, PDFs, and audio files.

---

## Q2. What does BLOB stand for?

**Answer**

Binary Large Object—a binary file stored as a single object.

---

## Q3. Should we store images directly in a relational database?

**Answer**

Usually no.

Large files are typically stored in object storage, while the database stores metadata such as the file name or URL.

---

## Q4. What is Object Storage?

**Answer**

Object storage stores files as independent objects together with metadata. It is highly scalable and designed for storing large amounts of unstructured data.

---

## Q5. What should be stored in MySQL and what should be stored in Amazon S3?

**Answer**

Store structured metadata (user ID, filename, URL, upload time) in MySQL, and store the actual binary file (image, video, PDF) in Amazon S3.

---

## Q6. What is the difference between MongoDB and Amazon S3?

| MongoDB | Amazon S3 |
|----------|-----------|
| Stores JSON-like documents | Stores binary objects/files |
| Query document fields | Retrieve objects by key |
| Semi-structured data | Unstructured file storage |