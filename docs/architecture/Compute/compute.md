# Compute

### **1. Compare EC2, Lambda, and Fargate — when would you use each?**

**Answer:**

* **EC2** gives full control over servers — OS, networking, and configurations.

  * ✅ Best for **long-running, predictable workloads**, **legacy systems**, or when you need **custom environments**.
* **Lambda** is **serverless compute** — runs code in response to events without managing servers.

  * ✅ Best for **short-lived, event-driven workloads** like APIs, automation, and data processing.
  * ⚠️ Has limits on execution time (max 15 minutes) and memory.
* **Fargate** runs **containers without managing servers** — a middle ground.

  * ✅ Best for **containerized microservices** or **batch jobs** where you want the flexibility of containers but no server management.

**In summary:**
EC2 → full control,
Lambda → serverless & event-driven,
Fargate → containers without servers.

---

### **2. What is the purpose of Auto Scaling Groups (ASGs), and how do they work?**

**Answer:**
Auto Scaling Groups automatically adjust the number of EC2 instances based on demand to maintain performance and optimize cost.

* Define **min**, **max**, and **desired capacity**.
* Monitor metrics (CPU, requests, or custom CloudWatch metrics).
* **Scale out** when load increases, **scale in** when it decreases.
* Replace **unhealthy instances automatically**.
* Often paired with a **Load Balancer** for even traffic distribution.

✅ **Benefits:** Scalability, fault tolerance, and cost efficiency.

---

### **3. How do you attach persistent storage to a Lambda function?**

**Answer:**
Lambda functions are **stateless**, so you must attach external persistent storage.

**Options:**

1. **Amazon EFS (Elastic File System):**

   * Mount a shared file system to your Lambda function (e.g. `/mnt/data`).
   * Use for ML models, shared files, or large read/write workloads.
2. **Amazon S3:**

   * Use AWS SDK to `GetObject` / `PutObject`.
   * Ideal for storing logs, files, and event-driven data.
3. **DynamoDB:**

   * For structured NoSQL data (user sessions, metadata, etc.).
4. **/tmp directory:**

   * 512 MB ephemeral storage; cleared after execution.

✅ **Best choice:**

* Use **EFS** for file-based workloads.
* Use **S3** for object-based persistence.

---

### **4. When should you use EC2 Reserved Instances vs Spot Instances?**

**Answer:**

* **Reserved Instances (RIs):**

  * Commit to 1–3 years → up to 72% discount.
  * ✅ Use for **steady-state**, **mission-critical**, or **always-on workloads**.
  * Ensures **capacity reservation** and **cost predictability**.
* **Spot Instances:**

  * Use spare AWS capacity at up to 90% discount.
  * ✅ Use for **fault-tolerant**, **stateless**, or **batch workloads**.
  * ⚠️ Can be **interrupted with 2-minute warning**.

**Summary:**
Reserved → predictable workloads.
Spot → flexible, interruptible workloads.

---

### **5. How do you deploy EC2 instances across multiple regions?**

**Answer:**
You can’t move an instance directly; instead, **copy its image (AMI)** to other regions.

**Steps:**

1. **Create an AMI** from your EC2 instance.
2. **Copy the AMI** to target regions (AMIs are region-specific).
3. **Launch new instances** in each region using the copied AMI.

**Automation options:**

* **CloudFormation StackSets** → deploy to multiple regions/accounts.
* **CI/CD pipeline** → automate AMI creation and replication.

---

### **6. How can you scale up for predictable workloads and scale down afterward?**

**Answer:**
Use **Scheduled Scaling** with an **EC2 Auto Scaling Group**.

**Steps:**

1. Create an ASG (min, max, desired instances).
2. Add **scheduled actions**:

   * Scale **up** before peak (e.g. 11:30 AM → increase desired capacity).
   * Scale **down** after (e.g. 1:30 PM → decrease capacity).

✅ Ideal for predictable traffic patterns like office hours or daily spikes.

---

### **7. Explain the basics of Amazon ECS.**

**Answer:**
Amazon ECS (Elastic Container Service) manages and orchestrates Docker containers on AWS.

**Core Components:**

* **Task Definition:** Blueprint of your container setup (image, CPU/mem, ports).
* **Task:** Running instance of a task definition.
* **Service:** Ensures desired number of tasks are running; integrates with load balancer.
* **Cluster:** Logical grouping of tasks or EC2 instances.

**Launch Types:**

* **Fargate:** Serverless — AWS manages servers.

  * Pay for CPU/memory per task.
* **EC2:** You manage EC2 instances that run containers.

  * Gives control and cost optimization.

---

### **8. How does scaling work in ECS?**

**Answer:**
ECS supports two types of scaling:

**1. Service (Task) Scaling:**

* Scales **number of running containers**.
* Types of scaling:

  * **Target Tracking:** Maintain a metric (e.g. CPU at 70%).
  * **Step Scaling:** Incrementally scale based on thresholds.
  * **Scheduled Scaling:** Scale for predictable times.

**2. Cluster (Infrastructure) Scaling:**

* **Fargate:** AWS handles scaling automatically.
* **EC2:** You use an **Auto Scaling Group** to manage EC2 capacity based on ECS metrics.

---

### **9. How do you set up scheduled scaling in ECS?**

**Answer:**
Same idea as EC2 scheduled scaling — but applied to ECS tasks.

**Steps:**

1. Create an **ECS Service** (defines task and initial desired count).
2. Enable **Service Auto Scaling** (set min, max, desired).
3. Define **scheduled actions**:

   * **Scale Up:** e.g. increase task count at 11:30 AM.
   * **Scale Down:** e.g. decrease task count at 1:30 PM.

✅ Useful for predictable workloads like daily traffic spikes.

---

### **10. How does load balancing work in ECS?**

**Answer:**
ECS integrates with **Elastic Load Balancing (ELB)** — typically **Application Load Balancer (ALB)** for web apps.

**Key Components:**

1. **Load Balancer (ALB):** Routes incoming traffic.
2. **Target Group:** Registered ECS tasks (containers).
3. **ECS Service:** Automatically registers/deregisters tasks with the target group.

**Flow:**

* User → ALB DNS name → healthy task in Target Group → response back.

✅ Ensures even traffic distribution and automatic health management.

---

### **11. How does Lambda handle scaling compared to ECS and EC2?**

**Answer:**
Lambda **scales automatically and instantly** — each event triggers a new isolated function instance.
No manual configuration of scaling policies.

* **Scaling Unit:** Function invocations.
* **Concurrency Limit:** Default 1,000 per region (can increase).
* **Cost Model:** Pay per execution time and requests.

✅ Best for event-driven workloads.
⚠️ Not suitable for long-running or stateful workloads.
