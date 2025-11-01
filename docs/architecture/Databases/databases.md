# Database

## DynamoDB

### Q: How does DynamoDB scale?
A: DynamoDB scales automatically by distributing data and traffic across multiple partitions to handle increasing workloads. It uses **horizontal scaling**, SSDs, and a shared-nothing architecture to deliver low-latency performance at any scale.

#### 1. Horizontal Scaling 📈
- DynamoDB adds more servers (partitions) rather than scaling vertically.
- Each table starts with a number of partitions based on provisioned capacity or on-demand usage.
- When data or traffic exceeds a partition's limit, **partition splitting** occurs automatically to avoid bottlenecks.

#### 2. Partition Keys and Partitions 🔑
- Data is distributed based on the **partition key** using an internal hash function.
- **Uniform Distribution**: High-cardinality and evenly accessed keys are essential for scalability.
- **Hot Partitions**: If some keys are accessed disproportionately, throttling can occur. Avoid this by spreading requests across multiple keys.

#### 3. On-Demand vs Provisioned Capacity ⚖️
- **On-Demand**: Automatically scales to handle traffic, pay per request.
- **Provisioned**: You set RCUs and WCUs. Can use **Auto Scaling** to adjust dynamically based on load.

---

### Q: How does DynamoDB scale writes?
A: DynamoDB distributes writes across **partitions**, scaling horizontally as traffic increases.

- **Partition Key**: Determines which partition stores an item. High cardinality and uniform access patterns ensure even distribution.
- **Capacity Modes**:
  - **On-Demand**: Automatically scales to handle sudden peaks.
  - **Provisioned with Auto Scaling**: Adjusts WCUs dynamically within defined min/max to prevent throttling.

#### Avoiding Hot Partitions 🔥
- Hot partitions happen when one key receives disproportionate traffic.
- **Solutions**: Use **write sharding** or **composite keys** (e.g., `productID-1`, `productID-2`) to spread writes evenly.

---

### Q: How does DynamoDB prevent race conditions?
A: DynamoDB offers multiple mechanisms to prevent data corruption when multiple writers update the same item:

1. **Conditional Writes (Optimistic Concurrency Control)**
   - Use `ConditionExpression` to ensure a write only succeeds if a condition holds (e.g., version check).
   - Ensures **last writer wins** only if the client has the latest version.

2. **Transactions (ACID)**
   - `TransactWriteItems` allows multiple items to be updated atomically with optional conditions.
   - Best for workflows requiring **all-or-nothing semantics**.

3. **Atomic Counters**
   - Increment/decrement numeric attributes atomically without reading first.
   - Combine with conditional writes for business logic validation.

4. **Idempotency Keys (Application-level)**
   - Prevent duplicate updates for operations like payments using stored idempotency tokens.

**Trade-offs**:
- Conditional writes → single-item concurrency.
- Transactions → multi-item consistency.
- Atomic counters → numeric updates.
- Default last-writer-wins → risky if overwrites matter.

✅ Short answer: **Yes, use conditional writes or transactions** to prevent corruption.

---

## Comparing RDS and DynamoDB

### Q: Explain the difference between RDS and DynamoDB.
A:
- **RDS**: Managed relational database, SQL-based (MySQL, PostgreSQL, SQL Server). Ideal for structured data, complex queries, joins, and transactional workloads.
- **DynamoDB**: Managed NoSQL database, key-value/document model. Ideal for high throughput, low-latency workloads, and horizontal scalability. Great for user sessions, IoT data, leaderboards, and unpredictable workloads.

---

### Q: When would you choose DynamoDB vs RDS?
A:
- **Choose DynamoDB**: Need fully managed NoSQL with high scalability and low latency, flexible schema, unpredictable traffic (e.g., session storage, real-time data).
- **Choose RDS**: Structured data, complex queries, joins, strong transactional consistency (e.g., healthcare apps needing relational data).

---

## Aurora vs RDS

### Q: What is the difference between AWS Aurora and RDS?
A:
| Feature | AWS Aurora | Amazon RDS |
| --- | --- | --- |
| Architecture | Distributed, cloud-native, storage replicated across 3 AZs | Traditional, single-instance (replicas optional) |
| Performance | Up to 5x faster than MySQL, 3x faster than PostgreSQL | Comparable to standard open-source engines |
| Pricing | Higher instance cost, pay-for-use storage | Cost-effective for smaller workloads |
| Fault Tolerance | Auto-replicated, self-healing, high availability | Multi-AZ optional for high availability |
| Scaling | Storage scales automatically up to 128 TB; compute can scale independently | Storage pre-provisioned; vertical scaling needed |

**Use Aurora when**: high performance, high write load, superior fault tolerance, unpredictable workloads.  
**Use RDS when**: small/medium apps, budget-conscious, unsupported Aurora engines, lift-and-shift scenarios.

---

### Q: How do we scale up RDS instances horizontally?
A:
- **Horizontal scaling** is only for reads via **Read Replicas**.
  - Read replicas get asynchronous copies of the primary DB.
  - Distribute read queries to replicas for scalability.
- **Write scaling** is vertical (larger instance class) or using Aurora/DynamoDB.
- **Multi-AZ** is for high availability, not read scaling.

---

### Q: How do I make RDS globally available?
A:
1. **Aurora Global Database**
   - Primary cluster handles writes; up to 5 read-only secondary clusters in other regions.
   - High-speed replication, low-latency reads, near-zero RPO/RTO for disaster recovery.
2. **Cross-Region Read Replicas**
   - Asynchronous replication to other regions.
   - Manual promotion required for disaster recovery.
   - Simpler but less performant than Aurora Global Database.

---

### Q: How does Aurora scale writes?
A:
1. **Single Writer Model ✍️**
   - One primary instance handles writes.
   - Scaling is vertical (larger instance class).

2. **Multi-Writer ✍️✍️**
   - Multiple instances handle writes simultaneously (Aurora MySQL only).
   - Challenges: conflict management, no dedicated read replicas, single-region, not for high-contention workloads.

3. **Aurora Serverless v2 🚀**
   - Auto-scales compute in fractions of a second.
   - Provides elastic vertical scaling for sudden spikes.

---

### Q: Is there a limit to how much Aurora Serverless can scale up?
A:
- Maximum **256 ACUs** (Aurora Capacity Units).
- Scaling occurs in increments of 0.5 ACUs.
- Scaling beyond max capacity is **throttled**, preventing unexpected costs.
