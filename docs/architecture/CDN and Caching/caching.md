# CDN and Caching

## AWS CloudFront

### Q: How does CloudFront improve application performance?

**A:** CloudFront is AWS’s CDN that caches content at **edge locations** close to users, reducing latency and improving load times.

```jsx
Request -> DNS -> CloudFront -> API Gateway -> Backend Server
```

* Caches static assets (images, CSS, JS) and can accelerate dynamic content.
* Optimizes network routes for faster content delivery.
* Offloads traffic from origin servers.
* Integrates with AWS Shield & WAF to protect against DDoS and exploits.

---

### Q: How does CloudFront know what to cache?

**A:** CloudFront uses a **cache key** and **cache policies** to determine what to cache.

**Cache Key Components**:

* URL path (e.g., `/images/logo.png` vs `/css/style.css`)
* Optional components:

  * Query strings (`?version=1`)
  * HTTP headers (`Accept-Language`)
  * Cookies (user-specific content)

**Cache Policies & Behaviors**:

* **Cache Policy**: Defines which parts of a request are included in the cache key and TTL.
* **Cache Behaviors**: Apply different caching rules for different URL patterns (e.g., long TTL for static assets, short/no cache for `/api/*`).

---

### Q: What security features does CloudFront provide?

**A:** CloudFront secures applications at the edge before requests reach your backend.

**Core Features**:

* **DDoS Protection** 🛡️: Integrates with AWS Shield Standard (and Advanced)
* **AWS WAF** 🧱: Filters SQL injection, XSS, and other threats
* **HTTPS/TLS Encryption** 🔒: Enforces secure connections to edge and origin
* **Origin Access Control (OAC)** 🔐: Restricts S3 access to CloudFront only
* **Signed URLs & Cookies**: For time-limited access to private content
* **Field-Level Encryption**: Encrypt sensitive HTTP request data at the edge
* **Geo-Restriction**: Restrict content access by country

---

## AWS Storage Options

### Q: What are the differences between S3, EFS, and EBS?

**A:**

| Service | Type                | Use Case                                                                                       |
| ------- | ------------------- | ---------------------------------------------------------------------------------------------- |
| **S3**  | Object storage      | Static assets, backups, images/videos, write-once read-many workloads                          |
| **EBS** | Block storage       | EC2-attached virtual disks for databases, file systems needing low-latency, persistent storage |
| **EFS** | Network file system | Shared file storage across multiple EC2 instances, auto-scaling, multi-AZ                      |

> Block storage stores raw data in fixed-size blocks; the OS or application manages files on top. Object storage (S3) stores entire objects accessible via API.

---

### Q: Are S3 buckets globally available?

**A:** No, S3 buckets are **regional**. Data stays in the selected Region unless explicitly replicated.

**To make data globally available**:

* **Cross-Region Replication (CRR)**: Async replication to another Region
* **S3 Multi-Region Access Points**: Single global endpoint routing to nearest bucket
* **CloudFront**: Caches S3 objects at edge locations for low-latency delivery

---

## AWS ElastiCache

### Q: What is Amazon ElastiCache?

**A:** ElastiCache is a fully managed **in-memory caching service** that improves application performance by reducing database hits.

**How it Works**:

* Application checks cache first:

  * ✅ Cache hit → fast memory access
  * ❌ Cache miss → fetch from DB, store in cache
* Reduces database load and improves responsiveness.

**Common Use Cases**:

* Session storage
* Frequently-read queries
* Real-time analytics
* Leaderboards/counters

**Engines**: Redis (advanced features) vs Memcached (simpler key-value cache)

---

### Q: How does ElastiCache handle scalability and availability?

**A:**

**Scalability**

* **Horizontal Scaling**: Add nodes

  * Redis Cluster Mode shards data across nodes
  * Read replicas distribute read traffic
* **Vertical Scaling**: Resize instance type
* **Auto Scaling**: Automatically adjusts based on CPU/memory usage

**Availability**

* **Multi-AZ with Automatic Failover**: Promotes replicas automatically if primary fails
* **Cluster Mode Enabled**: Shards distributed across AZs for resilience
* **Backups & Snapshots**: Daily automated backups
* **Global Datastore**: Multi-region replication for low-latency reads & disaster recovery

---

### Q: What are the differences between Memcached and Redis?

| Feature         | Memcached              | Redis                                                     |
| --------------- | ---------------------- | --------------------------------------------------------- |
| Primary Purpose | Simple in-memory cache | In-memory cache + data store                              |
| Data Structures | Strings only           | Strings, Hashes, Lists, Sets, Sorted Sets, Streams        |
| Persistence     | ❌ No                   | ✅ Optional (RDB/AOF)                                      |
| Replication/HA  | ❌ None                 | ✅ Master-replica, clustering, Sentinel                    |
| Eviction Policy | LRU                    | Multiple options (LRU, LFU, TTL)                          |
| Transactions    | ❌ No                   | ✅ Yes (MULTI/EXEC, Lua)                                   |
| Performance     | Very lightweight       | Slightly heavier, still fast                              |
| Scalability     | Client-side sharding   | Native clustering & automatic sharding                    |
| Use Cases       | Simple caching         | Caching + pub/sub, leaderboards, queues, locks, analytics |

**When to Use Memcached**

* Simple key-value caching
* Extremely low-latency, high throughput
* No persistence/replication needed

**When to Use Redis**

* Need persistence, HA, rich data types
* Sessions, leaderboards, distributed locks, queues, rate-limiting

**Interview Soundbite**:

> “Memcached is lightweight and simple, Redis is feature-rich with persistence, replication, clustering, and pub/sub. Use Memcached for ephemeral caching, Redis for durability or complex features.”
