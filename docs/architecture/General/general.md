# AWS Architecture & System Design Notes

## 🏗️ Designing Scalable and Highly Available Applications

### **How would you design a scalable web app on AWS?**

To design a scalable web application on AWS, I would start by separating the architecture into layers — front-end, application layer, and data layer — and use fully managed, scalable services where possible.

* **Front-end**: Host static content (HTML, CSS, JS) in an **S3** bucket and use **CloudFront** as a CDN to cache content globally and reduce latency.
* **Backend**:

  * For serverless workloads → **API Gateway + Lambda** (auto-scaling, no infrastructure).
  * For containerized workloads → **ECS** or **EKS** behind an **Application Load Balancer**, enabling horizontal scaling.
* **Database**:

  * Relational → **RDS** with Multi-AZ.
  * NoSQL → **DynamoDB** with high throughput.
  * Add **ElastiCache** to reduce database load.
* **Decoupling**: Use **SQS** or **SNS** to handle asynchronous tasks and maintain responsiveness during spikes.
* **Scaling & Monitoring**:

  * **Auto Scaling Groups** or **Fargate** for compute.
  * **CloudWatch** for monitoring.
  * **WAF** and **Shield** for security.

> The goal is elasticity — scaling up during peak times and scaling down during low traffic to save cost.

---

### **How would you design a highly available web application on AWS?**

To design a highly available application, I’d focus on eliminating single points of failure by distributing across multiple **Availability Zones**.

* **Front-end**:

  * Host static assets in **S3**, cache via **CloudFront**.
  * Use **Route 53** with health checks and **ALB** for load distribution.
* **Compute**:

  * **EC2** with Auto Scaling Groups across 2+ AZs.
  * Or use **ECS/EKS** with **Fargate** for managed scaling.
  * For event-driven workloads → **Lambda**.
* **Database Layer**:

  * Relational → **RDS** with Multi-AZ.
  * NoSQL → **DynamoDB** with on-demand capacity and global tables.
* **Resilience Enhancements**:

  * **ElastiCache** for caching.
  * **SQS** for decoupling components.
* **Monitoring & Security**:

  * **CloudWatch**, **AWS WAF**, and **Route 53** health checks.

> High availability means designing for redundancy, auto-scaling, and fast recovery.

---

### **What’s the difference between vertical and horizontal scaling?**

* **Vertical Scaling (Scale Up)** → Increase instance capacity (CPU, memory, storage).

  * Example: EC2 `t3.medium → m6i.large`.
  * Easier to implement but limited and introduces single-point risk.
* **Horizontal Scaling (Scale Out)** → Add more instances or nodes.

  * Example: More EC2s behind a Load Balancer, or more ECS tasks/pods.

> Horizontal scaling is preferred in AWS — supports high availability and elasticity.

---

### **How do you design for fault tolerance in AWS?**

Fault tolerance means the system keeps operating even if components fail.

* **Compute**: Distribute EC2 instances or containers across multiple AZs behind an **ALB**.
* **Database**:

  * **RDS** Multi-AZ,
  * **DynamoDB** (multi-AZ by default),
  * **Aurora Global Database** for cross-region replication.
* **Decoupling**: Use **SQS** or **EventBridge** to isolate failures.
* **Auto Recovery**:

  * **Auto Scaling Groups** to replace unhealthy instances.
  * **CloudWatch** for alerts and **Route 53** health checks.

> Build redundancy at every layer — compute, storage, network, and database.

---

### **How would you scale a relational database?**

#### 🧠 Sample Answer

Relational databases can be scaled both vertically and horizontally depending on workload.

---

#### 1. **Vertical Scaling (Scale Up)**

* Increase instance size (CPU, memory, IOPS).
* Quick and simple, but limited by hardware.
  ✅ Example: Upgrade `db.t3.medium → db.m6i.2xlarge`

---

#### 2. **Read Scaling (Horizontal Read Scaling)**

* Add **read replicas** (RDS/Aurora).
* Use read/write split via app logic or **RDS Proxy**.
  ✅ Good for analytics, dashboards, search.

---

#### 3. **Caching Layer**

* Use **ElastiCache (Redis/Memcached)** to cache hot data.
  ✅ Greatly reduces DB load.

---

#### 4. **Query Optimization & Indexing**

* Analyze queries using **Performance Insights** and logs.
* Add proper indexes and fix N+1 queries.
  ✅ Low-cost, high-impact.

---

#### 5. **Partitioning / Sharding**

* Split DB by region, tenant, or data type.
  ✅ Example: Separate DB per customer region.

---

#### 6. **Migration to Aurora or Serverless**

* **Aurora Serverless v2** can auto-scale compute dynamically.

✅ Ideal for unpredictable workloads.

---

#### ✅ Summary

> Start with vertical scaling, then add read replicas and caching.
> Optimize queries before considering complex options like sharding or Aurora Serverless.

---

### **What’s the difference between scalability and elasticity?**

* **Scalability** → System’s ability to handle more workload by adding resources (manual or automated).
* **Elasticity** → Automatic scaling **up and down** based on real-time demand.

✅ Examples:

* Scalability: Add more EC2 instances.
* Elasticity: Auto Scaling Group adds/removes instances automatically.

> Scalability = capacity to grow.
> Elasticity = dynamic right-sizing.

---

### **What is a CDN and when would you use it?**

A **Content Delivery Network (CDN)** is a globally distributed network of edge servers that cache and serve content close to users.

* Reduces latency and improves load times.
* Offloads traffic from origin servers.
* Ideal for static assets: images, videos, JS, CSS.

✅ In AWS, use **CloudFront**.

---

### **What happens when a user enters “google.com” in their browser?**

1. **URL Parsing** – Extract protocol and domain.
2. **DNS Lookup** – Resolve domain → IP address.
3. **TCP Connection** – 3-way handshake over port 443 (HTTPS).
4. **TLS Handshake** – Establish secure session.
5. **HTTP Request** – Browser sends a `GET` request.
6. **Server Response** – Returns HTML + assets.
7. **Rendering** – Browser parses and renders content.
8. **CDN Role** – Delivers cached assets (images, CSS, JS) via edge servers.

---

### **How does CloudFront know what to cache?**

1. **Cache Behavior Settings** – Path patterns, methods, cookies, etc.
2. **HTTP Cache-Control Headers** – Origin defines TTL rules.
3. **Time-to-Live (TTL)** – Minimum, Default, Maximum TTL values.
4. **Cache Key Config** – Determines uniqueness (query, headers, cookies).

---

## 🧩 Microservices Architecture

### **What is Microservice Architecture?**

Microservice architecture structures software as a collection of small, **independently deployable** services — each responsible for a single business function.

#### 🔧 Key Characteristics

* **Single Responsibility** – Each service has one function.
* **Independent Deployment** – No need to redeploy the whole system.
* **Tech Flexibility** – Different stacks per service.
* **Resilience** – Faults isolated to a single service.
* **DevOps Friendly** – Works well with CI/CD, containers, ECS/EKS.

#### 🧩 AWS Context

> Commonly built using ECS, EKS, or Lambda.
> Services communicate via API Gateway, EventBridge, or SQS/SNS.

---

### **How would you expose microservices to external clients?**

* Use **API Gateway** or an **Application Load Balancer** as the entry point.

---

### **How do you ensure scalability and high availability of microservices on AWS?**

* Use **Auto Scaling Groups**, **ECS/EKS with Fargate**, and **Load Balancing** across multiple AZs.

---

### **Data Management**

* **How do you manage data consistency?**
  → Use **eventual consistency** or the **Saga Pattern**.

* **What is the database-per-service pattern?**
  → Each service owns its database to ensure loose coupling and independence.

---

### **Observability**

* **Monitoring** → CloudWatch, X-Ray, OpenTelemetry.
* **Tracing** → Use correlation IDs or distributed tracing across services.

---

### **Security**

* **Service Communication** → Use **TLS**, **mutual TLS**, **IAM roles**, **JWT tokens**.
* **Authentication/Authorization** → Centralized identity management (e.g., Cognito, Auth0).

---

### **Deployment Strategies**

* **Deployments** → CI/CD (e.g., GitLab, CodePipeline).
* **Strategies** → Blue/Green, Rolling, or Canary.
* **Zero Downtime** → ECS rolling updates or CodeDeploy integration.

---

### **How would you scale containers hosted in EKS?**

* Use **Horizontal Pod Autoscaler (HPA)** to adjust pod count based on CPU/memory metrics.
* Use **Cluster Autoscaler** to scale worker nodes dynamically.
* Combine with **AWS Fargate** for serverless pod scaling.

