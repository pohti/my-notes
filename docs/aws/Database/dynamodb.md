# DynamoDB

## 🔹 What is DynamoDB?

**Amazon DynamoDB** is a **fully managed NoSQL database** service by AWS that provides:

* **Key-value and document data model**
* **Single-digit millisecond performance**
* **Automatic scaling**, replication, and high availability.

**Use cases:** real-time apps, gaming, IoT, session stores, e-commerce carts, serverless backends.

---

## ⚙️ Core Concepts

| Concept         | Description                                                                |
| --------------- | -------------------------------------------------------------------------- |
| **Table**       | A collection of items (like a table in RDBMS).                             |
| **Item**        | A single record in a table (like a row).                                   |
| **Attribute**   | A key-value pair in an item (like a column).                               |
| **Primary Key** | Uniquely identifies an item — can be simple or composite.                  |
| **Partition**   | The physical storage unit. Data is distributed based on the partition key. |

---

## 🧱 Primary Key Types

| Type                      | Structure                 | Example                         |
| ------------------------- | ------------------------- | ------------------------------- |
| **Simple Primary Key**    | Partition key only.       | `UserID = 123`                  |
| **Composite Primary Key** | Partition key + Sort key. | `UserID = 123`, `OrderID = 456` |

🔹 **Partition key** decides the partition placement.
🔹 **Sort key** allows range queries (e.g. fetch all orders for a user).

---

## ⚡ Read & Write Capacity Modes

| Mode                 | Description                                                          | Best for                             |
| -------------------- | -------------------------------------------------------------------- | ------------------------------------ |
| **Provisioned Mode** | You define RCU (Read Capacity Units) and WCU (Write Capacity Units). | Predictable workloads.               |
| **On-Demand Mode**   | Scales automatically, pay-per-request.                               | Unpredictable workloads or new apps. |

**1 RCU = 1 strongly consistent read/sec (4 KB)**
**1 WCU = 1 write/sec (1 KB)**

---

## 🧩 Indexes

Indexes enable fast queries beyond the primary key.

| Type                             | Description                                                                                |
| -------------------------------- | ------------------------------------------------------------------------------------------ |
| **Local Secondary Index (LSI)**  | Same partition key, different sort key. Created **at table creation**.                     |
| **Global Secondary Index (GSI)** | Different partition and sort key. Created **anytime**. Supports queries across partitions. |

---

## 🔄 Streams

**DynamoDB Streams** capture **data modification events** (insert, update, delete).
Commonly used for:

* Triggering **AWS Lambda functions**
* **Change data capture (CDC)** pipelines
* **Cross-region replication**

---

## 🧠 Query vs Scan

| Operation | Description                                    | Performance             |
| --------- | ---------------------------------------------- | ----------------------- |
| **Query** | Retrieves items based on primary key or index. | Fast (uses indexes).    |
| **Scan**  | Reads every item in a table.                   | Slow (full table read). |

👉 Always prefer **Query** over **Scan** for efficiency.

---

## 🔐 Security

* **Encryption at rest:** Enabled by default (AWS KMS).
* **Encryption in transit:** TLS.
* **IAM policies:** Fine-grained access control (per table or per item).
* **Condition keys:** Enforce row-level security.
* **Point-in-time recovery (PITR):** Restore table to any time in the last 35 days.

---

## 🧰 Integrations

| Service                   | Integration                                        |
| ------------------------- | -------------------------------------------------- |
| **AWS Lambda**            | React to data changes via DynamoDB Streams.        |
| **API Gateway**           | Expose DynamoDB via RESTful APIs.                  |
| **CloudWatch**            | Monitor read/write throttling, latency, and usage. |
| **Glue / Athena**         | Query data using SQL for analytics.                |
| **Kinesis Data Firehose** | Stream DynamoDB changes to S3 or Redshift.         |

---

## 📦 Data Modeling Tips

1. **Design for access patterns**, not relationships (NoSQL principle).
2. Use **composite keys** to support 1-to-many relationships.
3. **Denormalize** data (duplicate where necessary).
4. Use **GSIs** for alternative query patterns.
5. Store **related data together** for efficient querying.

Example:

```json
{
  "PK": "USER#123",
  "SK": "ORDER#456",
  "OrderDate": "2025-10-29",
  "Total": 120.50
}
```

→ You can fetch all orders for a user with `PK = USER#123`.

---

## 💰 Pricing

You pay for:

* **Read/write capacity** (on-demand or provisioned)
* **Storage**
* **Streams reads**
* **Backup and restore**
* **Data transfer**

---

## 📊 Backup & Restore

* **On-demand backup:** Full backup anytime.
* **Point-in-time recovery (PITR):** Continuous backup for last 35 days.
* **Cross-region replication:** Via **Global Tables**.

---

## 🌍 Global Tables

Provide **multi-region, active-active replication**.
Changes in one region are replicated automatically to others.

**Use case:** Global applications needing low latency & regional redundancy.

---

## 🔄 Caching with DAX

**DynamoDB Accelerator (DAX)** = in-memory cache for DynamoDB.

* Fully managed, compatible with DynamoDB API.
* Reduces read latency from milliseconds → microseconds.
* Ideal for read-heavy workloads.

---

## 🧩 Common Interview Questions

| Question                                     | Key Points to Mention                                                                                            |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **What is DynamoDB?**                        | Fully managed NoSQL key-value/document DB.                                                                       |
| **DynamoDB vs RDS?**                         | NoSQL vs SQL; schema-less; scales horizontally.                                                                  |
| **Difference between Query and Scan?**       | Query uses keys/indexes; Scan reads all items.                                                                   |
| **Explain GSI and LSI.**                     | GSI: different partition key, can be added later. LSI: same partition key, different sort key, only at creation. |
| **How does DynamoDB ensure scalability?**    | Auto-partitioning based on partition key hash.                                                                   |
| **What are WCU and RCU?**                    | Capacity units for provisioned throughput.                                                                       |
| **What is DAX?**                             | In-memory cache for low-latency reads.                                                                           |
| **What is DynamoDB Streams?**                | Change logs for triggering events or replication.                                                                |
| **How do you handle backups?**               | PITR and on-demand backups.                                                                                      |
| **How does DynamoDB integrate with Lambda?** | Lambda triggers from Streams.                                                                                    |
| **What are Global Tables?**                  | Multi-region active-active replication.                                                                          |
| **When would you choose On-Demand mode?**    | For unpredictable or bursty workloads.                                                                           |

---

## 🧠 Best Practices

✅ Choose **high-cardinality partition keys** to avoid “hot” partitions.
✅ Prefer **On-Demand** for unpredictable workloads.
✅ Use **DAX** for caching reads.
✅ Enable **PITR** for data protection.
✅ Use **IRSA** for Lambda/DynamoDB permissions.
✅ Monitor **throttled requests** in CloudWatch.

