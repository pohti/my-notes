# RDS

## 🧩 AWS RDS — Interview Notes

### 🔹 What is Amazon RDS?

Amazon RDS (Relational Database Service) is a **fully managed relational database service** that automates setup, operation, and scaling of databases in the cloud.
It handles administrative tasks like **provisioning, backups, patching, monitoring, and scaling**, so developers can focus on application logic instead of database management.

**Supported database engines:**

* Amazon Aurora
* PostgreSQL
* MySQL
* MariaDB
* Oracle
* Microsoft SQL Server

---

### ⚙️ Key Features

1. **Automated backups** – daily snapshots with point-in-time recovery.
2. **Multi-AZ deployments** – automatic failover for high availability.
3. **Read replicas** – for horizontal read scaling.
4. **Storage auto-scaling** – increases storage when needed.
5. **Security** – encryption, IAM integration, VPC isolation, and SSL.
6. **Monitoring** – CloudWatch metrics, Performance Insights, and Enhanced Monitoring.

---

### 🧱 Core Components

* **DB Instance** – a single database environment (runs one DB engine).
* **DB Cluster** – used by Aurora; multiple DB instances sharing one cluster volume.
* **Read Replica** – a read-only copy of a database for read-heavy workloads.
* **Parameter Groups** – configuration templates that control DB engine settings.
* **Option Groups** – additional database features (e.g., Oracle TDE, SQL Server Agent).

---

### 🧩 Multi-AZ Deployment

* Provides **high availability** and **automatic failover**.
* RDS automatically creates a **standby replica** in another Availability Zone.
* The standby is continuously updated through **synchronous replication**.
* Failover occurs automatically during maintenance, AZ outage, or hardware failure.
* Used for **production workloads** that require reliability.

---

### ⚡ Read Replicas

* Used for **scaling read operations** (asynchronous replication).
* Can be promoted to a standalone DB instance if needed.
* Up to 5 read replicas per source (more for Aurora).
* Useful for analytics, reporting, and read-heavy APIs.
* Available within the same region or across regions.

---

### 🔐 Security

1. **Encryption at rest** – AWS KMS-managed keys.
2. **Encryption in transit** – via SSL/TLS.
3. **IAM integration** – manage access using roles and policies.
4. **VPC** – isolates RDS instances in private subnets.
5. **Security groups** – control inbound/outbound traffic.
6. **RDS Proxy** – adds secure connection pooling between applications and the database.

---

### 🧰 Backups and Recovery

* **Automated backups**: Daily backups retained for 1–35 days (includes transaction logs).
* **Manual snapshots**: User-initiated, stored until explicitly deleted.
* **Point-in-time recovery (PITR)**: Restore a database to any specific second within the retention window.
* **Cross-region snapshot copy**: For disaster recovery or compliance.
* **Restore options**: Create a new DB instance from a snapshot.

---

### 🌍 Storage and Scaling

* **Storage types**:

  * General Purpose (SSD)
  * Provisioned IOPS (high performance)
  * Magnetic (legacy)
* **Scaling methods**:

  * **Vertical scaling**: Modify the instance class (CPU, memory).
  * **Horizontal scaling**: Use read replicas.
  * **Storage auto-scaling**: Automatically increases allocated storage when near capacity.

---

### 📊 Monitoring and Performance

* **Amazon CloudWatch** – tracks metrics such as CPU, storage, IOPS, and connections.
* **Enhanced Monitoring** – OS-level metrics (RAM, processes).
* **Performance Insights** – identifies slow queries and performance bottlenecks.
* **Event notifications** – alerts for failovers, maintenance, or backup events.

---

### 💰 Pricing Model

You pay for:

1. Database instance hours (depends on class and region)
2. Allocated storage (GB per month)
3. I/O requests
4. Backups and snapshots
5. Data transfer (between regions or out of AWS)
6. Multi-AZ and read replicas (charged separately)

---

### 🧩 Maintenance and Updates

* AWS handles **OS and DB engine patching**.
* You can define a **maintenance window** for scheduled updates.
* Manual patching or version upgrades are also possible when needed.

---

### 🧠 Common Interview Questions

1. **What is RDS?**
   A managed relational database service that automates administrative tasks.

2. **What databases are supported by RDS?**
   MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Aurora.

3. **What is the difference between Multi-AZ and Read Replica?**

   * Multi-AZ: synchronous replication for high availability.
   * Read Replica: asynchronous replication for read scaling.

4. **How does RDS handle backups?**
   Automated daily backups and manual snapshots with PITR.

5. **What is RDS Proxy?**
   A connection pooling layer for managing many short-lived connections efficiently (useful for serverless apps).

6. **How do you secure an RDS instance?**
   Use IAM authentication, encryption, VPC isolation, and security groups.

7. **Can you stop/start RDS instances?**
   Yes, for cost savings in development or testing environments.

8. **How do you scale an RDS instance?**
   Vertically by changing instance class, or horizontally via read replicas.

9. **Difference between RDS and Aurora?**
   Aurora is an AWS-built, MySQL/PostgreSQL-compatible database with higher performance and auto-scaling storage.

10. **How to monitor RDS performance?**
    Use CloudWatch, Enhanced Monitoring, and Performance Insights.

---

### 🧠 Best Practices
<br/>✅ Always enable **Multi-AZ** for production workloads.
<br/>✅ Enable **automated backups** and **PITR** for recovery.
<br/>✅ Store credentials in **AWS Secrets Manager**, not in code.
<br/>✅ Use **RDS Proxy** for connection-heavy applications (especially with Lambda).
<br/>✅ Enable **encryption** both at rest and in transit.
<br/>✅ Use **parameter groups** to fine-tune performance.
<br/>✅ Monitor **throttling, connections, and I/O** in CloudWatch.
<br/>✅ Regularly apply **patches and version upgrades**.
