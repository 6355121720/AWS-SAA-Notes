Here are the complete, structured notes for:

---

# 🗂️ **Section 21 – Databases in AWS** (Stephane Maarek – AWS SAA-C03)

---

## 🔰 Why Use Managed Databases?

* Avoid infrastructure headaches like:

  * Server provisioning
  * OS patching
  * Backup and recovery
  * High availability
* AWS offers **fully managed**, **scalable**, **secure** database solutions.

---

## 📚 Two Main Types of Databases

| Type                       | Services              | Best For                                   |
| -------------------------- | --------------------- | ------------------------------------------ |
| **Relational (SQL)**       | RDS, Aurora           | Structured data & complex relationships    |
| **Non-Relational (NoSQL)** | DynamoDB, ElastiCache | Scalability, flexibility, high performance |

---

## 🧱 Relational Database Services

### 🔹 **Amazon RDS (Relational Database Service)**

* Supports: **MySQL**, **PostgreSQL**, **MariaDB**, **Oracle**, **SQL Server**
* Managed service: handles provisioning, patching, backups, HA (Multi-AZ)
* **Read Replicas**: for read scaling (same region or cross-region)
* **Automated Backups** and **Snapshots**

### 🔹 **Amazon Aurora**

* AWS's own database engine
* Compatible with **MySQL** and **PostgreSQL**
* Better performance & availability (up to 15 read replicas)
* Auto-scaling storage, distributed and fault-tolerant
* **Aurora Serverless** for on-demand usage

---

## ⚡ NoSQL (Non-Relational)

### 🔸 **Amazon DynamoDB**

* Key-Value and Document database
* Fully managed, serverless, low-latency at any scale
* **Single-digit millisecond performance**
* Supports **auto scaling**, **DAX** (in-memory cache), **Streams**, **TTL**, **Global Tables**

---

## 🔄 Caching

### 🔸 **Amazon ElastiCache**

* In-memory data store to improve application performance
* Supports **Redis** and **Memcached**
* Use cases: **Session stores**, **Leaderboard**, **Real-time analytics**
* Reduces load on databases like RDS & DynamoDB

---

## 🔎 Search Databases

### 🔸 **Amazon OpenSearch Service (formerly Elasticsearch Service)**

* Full-text search, log analytics, and real-time monitoring
* Scalable and integrates with Kibana dashboards
* **Use case**: analyzing logs, search capabilities for applications

---

## 🔁 Data Warehousing

### 🔸 **Amazon Redshift**

* **OLAP** (Online Analytical Processing) optimized
* Scalable **data warehouse**
* Fast queries over large datasets using **columnar storage**
* **Spectrum** allows querying S3 directly using SQL

---

## 💡 Choosing the Right AWS Database

| Use Case                        | Recommended Service           |
| ------------------------------- | ----------------------------- |
| Relational DB, SQL              | RDS or Aurora                 |
| Serverless relational           | Aurora Serverless             |
| Massive scale / flexible schema | DynamoDB                      |
| In-memory, ultra-fast access    | ElastiCache (Redis/Memcached) |
| Log and text search             | OpenSearch                    |
| Analytics / BI / OLAP           | Redshift                      |

---

Let me know if you'd like the deep dive into any one of these like RDS, Aurora, DynamoDB, or Redshift next!
