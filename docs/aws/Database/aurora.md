# Aurora

### 1. **Overview**

* **Aurora** is a **relational database engine** compatible with **MySQL** and **PostgreSQL**.
* It’s part of **Amazon RDS**, but designed for **high performance and scalability**.
* Up to **5x faster than MySQL** and **3x faster than PostgreSQL** on the same hardware.
* Fully **managed** by AWS — handles backups, patching, monitoring, and failover automatically.

---

### 2. **Aurora Architecture**

* **Cluster-based architecture**:

  * One **primary instance** (read/write).
  * Up to **15 read replicas** (read-only).
  * Shared **storage volume** across all instances in a cluster.
* **Storage is distributed and fault-tolerant**:

  * Data is automatically replicated **6 times across 3 Availability Zones (AZs)**.
  * **Continuous backup to S3**.
  * **Auto-healing**: detects and repairs corrupt data blocks automatically.
* **Compute and storage are decoupled**:

  * Storage layer grows automatically up to **128 TB per cluster**.

---

### 3. **Aurora vs RDS**

| Feature         | Aurora                               | RDS                                                    |
| --------------- | ------------------------------------ | ------------------------------------------------------ |
| Engine          | Aurora (MySQL/PostgreSQL compatible) | Multiple (MySQL, PostgreSQL, Oracle, SQL Server, etc.) |
| Performance     | 3–5x faster                          | Standard performance                                   |
| Storage scaling | Auto up to 128 TB                    | Manual or limited                                      |
| Replication     | Up to 15 read replicas               | Up to 5 read replicas                                  |
| Failover        | Sub-second to seconds                | Tens of seconds to minutes                             |
| Availability    | Multi-AZ built-in                    | Optional Multi-AZ                                      |
| Pricing         | Slightly higher                      | Lower                                                  |
| Custom engines  | No                                   | Yes                                                    |

---

### 4. **Aurora Replicas**

* **Read Replicas** share the same underlying storage volume — **no need to copy data**.
* **Replication lag** is typically **&lt;100ms**.
* Can serve read queries and be **promoted to primary** for failover.
* You can also **distribute replicas across AZs** for high availability.

---

### 5. **Aurora Global Database**

* Designed for **cross-region replication** with **low latency**.
* Enables **read scalability** across multiple AWS regions.
* Replicates data with **&lt;1 second lag** using **dedicated network infrastructure**.
* Use cases:

  * **Global applications** with local reads.
  * **Disaster recovery** and **regional failover**.

---

### 6. **Aurora Serverless**

* **Auto-scaling version of Aurora**.
* Automatically starts, stops, and scales compute capacity based on usage.
* **Pay per second** for the actual database capacity used.
* Ideal for:

  * **Infrequent or unpredictable workloads**.
  * **Development and testing environments**.

**Aurora Serverless v2** (recommended):

* Scales in fine-grained increments.
* Supports **multi-AZ**, **read replicas**, and **global databases**.
* Much lower latency during scaling events.

---

### 7. **Aurora Backups & Recovery**

* **Continuous backup to S3** — no performance impact.
* **Point-in-time recovery (PITR)** available.
* **Automatic snapshots** retained for the defined backup retention period (1–35 days).
* You can also create **manual snapshots** anytime.

---

### 8. **Aurora Security**

* **Encryption at rest** using KMS.
* **Encryption in transit** using SSL/TLS.
* **IAM integration** for authentication (MySQL/PostgreSQL compatible).
* **Network isolation** via VPC.
* **Automatic patching** and **version upgrades**.

---

### 9. **Performance Features**

* **Parallel query execution**: pushes query processing to the storage layer.
* **Aurora cache warming**: preserves cache during failover.
* **Cluster endpoints**:

  * **Writer endpoint** – directs traffic to primary instance.
  * **Reader endpoint** – load balances read traffic across replicas.
  * **Custom endpoint** – directs traffic to specific replica groups.

---

### 10. **High Availability & Failover**

* Aurora continuously monitors health of instances.
* If the **primary instance fails**, an Aurora replica is automatically promoted.
* **Failover typically completes within 30 seconds**.
* Multi-AZ design ensures no single point of failure.

---

### 11. **Monitoring**

* Integrated with **Amazon CloudWatch**, **Performance Insights**, and **Enhanced Monitoring**.
* Metrics include CPU, memory, connections, replication lag, and query performance.
* Can set **CloudWatch alarms** for metrics like replica lag or storage usage.

---

### 12. **Pricing**

* Charged for:

  * **Compute capacity (instance hours)**.
  * **Storage (per GB/month)**.
  * **I/O requests (per million)**.
  * **Backup storage** beyond the retention period.
  * **Cross-region replication** (if applicable).
* **Aurora Serverless** is billed per second of usage.

---

### 13. **Common Interview Questions**

1. **What is Aurora? How is it different from RDS?**
   → Aurora is a MySQL/PostgreSQL-compatible managed database offering higher performance, distributed storage, and built-in high availability.

2. **What is Aurora Serverless used for?**
   → For unpredictable or intermittent workloads, as it auto-scales and charges per usage.

3. **How does Aurora achieve high availability?**
   → Replicates data across 3 AZs (6 copies), automatic failover, shared storage, and multi-replica support.

4. **What is Aurora Global Database?**
   → Enables cross-region read scalability and disaster recovery with &lt;1s replication lag.

5. **What are the different endpoints in Aurora?**
   → Writer, Reader, and Custom endpoints for load balancing and routing.

6. **What happens during failover in Aurora?**
   → An Aurora replica is promoted to primary automatically with minimal downtime.
