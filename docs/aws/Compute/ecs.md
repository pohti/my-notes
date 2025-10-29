# ECS

## 🔹 1. Basics

### **Q1. What is Amazon ECS?**

**A:**
Amazon ECS (Elastic Container Service) is a **fully managed container orchestration service** that lets you run Docker containers on AWS **without managing servers** directly.
It supports both:

* **EC2 launch type** → containers run on your own EC2 instances.
* **Fargate launch type** → containers run **serverlessly** (no instance management).

---

### **Q2. What are the main components of ECS?**

| Component           | Description                                                                                                 |
| ------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Cluster**         | Logical group of tasks or services.                                                                         |
| **Task Definition** | Blueprint describing which container images to run and their settings (CPU, memory, env vars, ports, etc.). |
| **Task**            | A running instance of a task definition (like a pod in Kubernetes).                                         |
| **Service**         | Manages long-running tasks; ensures desired count and supports load balancing and auto scaling.             |
| **Container Agent** | Runs on EC2 instances to communicate with the ECS control plane.                                            |
| **Launch Type**     | Defines where to run containers — EC2 or Fargate.                                                           |

---

## 🔹 2. Launch Types

### **Q3. What are the ECS launch types?**

| Launch Type | Description                                      | Use Case                                               |
| ----------- | ------------------------------------------------ | ------------------------------------------------------ |
| **EC2**     | You manage EC2 instances (container hosts).      | Full control, predictable workloads.                   |
| **Fargate** | AWS manages compute; you only define CPU/memory. | Serverless containers, short-lived or burst workloads. |

---

### **Q4. ECS Fargate vs EC2 — what’s the difference?**

| Feature        | ECS EC2                             | ECS Fargate                      |
| -------------- | ----------------------------------- | -------------------------------- |
| Infrastructure | You manage EC2 instances            | AWS manages everything           |
| Scaling        | Manual or auto scaling groups       | Automatic scaling                |
| Cost           | Pay for EC2 instances               | Pay per vCPU and memory          |
| Networking     | Requires configuring ENIs and ports | Simplified ENI per task          |
| Best for       | Long-running workloads              | On-demand, serverless containers |

---

## 🔹 3. Tasks and Services

### **Q5. What is a Task Definition in ECS?**

**A:**
A **Task Definition** is a JSON file that defines:

* Container image(s)
* CPU & memory requirements
* Environment variables
* Networking mode
* Logging configuration
* IAM roles

Example (simplified):

```json
{
  "family": "my-web-app",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:latest",
      "portMappings": [{ "containerPort": 80 }]
    }
  ]
}
```

---

### **Q6. What is a Task vs. a Service?**

| Concept     | Description                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------- |
| **Task**    | A single running copy of a Task Definition (can be standalone).                                                 |
| **Service** | Ensures a specified number of tasks are always running (handles restarts, load balancing, and rolling updates). |

---

### **Q7. How does ECS handle scaling?**

**A:**

* **Service Auto Scaling** — adjusts task count based on CloudWatch metrics.
* **Cluster Auto Scaling (for EC2)** — adds/removes EC2 instances as needed.
* **Fargate** automatically handles infrastructure scaling.

---

## 🔹 4. Networking

### **Q8. What are ECS networking modes?**

| Mode       | Description                                                              |
| ---------- | ------------------------------------------------------------------------ |
| **bridge** | Default Docker networking on EC2. Containers share host network via NAT. |
| **host**   | Containers share the host’s network interface (better performance).      |
| **awsvpc** | Each task gets its own ENI and private IP (default for Fargate).         |

---

### **Q9. How does ECS integrate with load balancers?**

**A:**
ECS integrates with **Elastic Load Balancing (ELB)**:

* **Application Load Balancer (ALB)** — layer 7 routing, path-based routing for web apps.
* **Network Load Balancer (NLB)** — layer 4 routing, high performance and static IPs.
* ECS automatically registers/deregisters tasks with the load balancer.

---

## 🔹 5. Storage & Data

### **Q10. How do containers persist data in ECS?**

**A:**

1. **EFS volumes** – shared persistent storage between tasks.
2. **EBS volumes** – attached to EC2 hosts.
3. **Ephemeral storage** – `/tmp` storage deleted when the task stops.

---

## 🔹 6. Security

### **Q11. What IAM roles are used in ECS?**

| Role               | Purpose                                                             |
| ------------------ | ------------------------------------------------------------------- |
| **Task Role**      | Grants AWS permissions *to the container itself* (e.g., access S3). |
| **Execution Role** | Used by ECS agent to pull images and write logs to CloudWatch.      |

---

### **Q12. How do you secure ECS containers?**

**A:**

* Use IAM roles for least privilege.
* Use **AWS Secrets Manager** or **SSM Parameter Store** for secrets.
* Limit network exposure via **Security Groups** and **private subnets**.
* Keep images small and scanned for vulnerabilities.

---

## 🔹 7. Monitoring & Logging

### **Q13. How do you monitor ECS?**

**A:**

* **CloudWatch Metrics** — CPU, memory, task count.
* **ECS Event Stream** — task start/stop events.
* **AWS X-Ray** — distributed tracing.
* **Container Insights** — per-container metrics.

---

### **Q14. How does ECS handle logging?**

**A:**
Containers send logs to:

* **CloudWatch Logs** (via `awslogs` driver)
* **FireLens** (custom log routing to Fluent Bit, Elasticsearch, etc.)
* Local file system or third-party logging services

Example task definition log config:

```json
"logConfiguration": {
  "logDriver": "awslogs",
  "options": {
    "awslogs-group": "/ecs/my-app",
    "awslogs-region": "us-east-1",
    "awslogs-stream-prefix": "ecs"
  }
}
```

---

## 🔹 8. Integration

### **Q15. How does ECS integrate with ECR?**

**A:**
ECS can pull container images directly from **Amazon Elastic Container Registry (ECR)** — AWS’s managed Docker registry.
You grant permissions using an **IAM role or policy**.

---

### **Q16. ECS vs EKS — what’s the difference?**

| Feature     | ECS                   | EKS                                    |
| ----------- | --------------------- | -------------------------------------- |
| Type        | AWS native            | Kubernetes-based                       |
| Complexity  | Simpler               | More complex                           |
| Control     | AWS-managed           | You manage Kubernetes control plane    |
| Portability | AWS-only              | Multi-cloud or on-prem supported       |
| Use Case    | Simpler microservices | Advanced orchestration or hybrid setup |

---

## 🔹 9. Deployment & Updates

### **Q17. How do you deploy updates in ECS?**

**A:**

* Update the **task definition revision** with a new image tag.
* Update the **service** to use the new revision.
* ECS performs **rolling updates** (replace old tasks gradually).

---

### **Q18. What is blue/green deployment in ECS?**

**A:**

* Managed through **AWS CodeDeploy**.
* Spins up a new environment (“green”) while keeping the old one (“blue”) running.
* After validation, traffic is switched to green.

---

## 🔹 10. Troubleshooting

### **Q19. Why might an ECS task fail to start?**

**Possible causes:**

* Invalid task definition.
* Insufficient CPU/memory.
* Missing IAM execution role permissions.
* Network misconfiguration (no ENI).
* Image pull failed from ECR/Docker Hub.

---

### **Q20. How do you debug ECS task issues?**

**A:**

* Check **ECS Console → Tasks → Logs**.
* View **CloudWatch Logs**.
* Inspect **Events tab** for failure messages.
* Use `aws ecs describe-tasks` for deeper insights.
* Use **AWS CloudTrail** for API failures.

---

## 🔹 11. Cost Optimization

### **Q21. How do you optimize ECS costs?**

**A:**

* Use **Fargate Spot** for non-critical workloads.
* Use **auto scaling** to run only necessary tasks.
* Consolidate containers efficiently on EC2 instances.
* Use **Graviton-based** EC2 for cheaper compute.
* Offload static assets to **S3 + CloudFront**.

---

## 🔹 12. Real-world Scenarios

### **Q22. When would you choose ECS over EKS?**

**A:**

* When you want a **fully managed AWS-native container solution**.
* You don’t need Kubernetes-level portability or complexity.
* Your workloads already run on AWS and use other AWS services.

---

### **Q23. Can ECS run multiple containers in one task?**

**A:**
Yes — a task definition can define **multiple containers**, useful for:

* Sidecar patterns (e.g., app + log forwarder)
* Shared storage via local volumes

---

### **Q24. How does ECS ensure high availability?**

**A:**

* Distributes tasks across **multiple Availability Zones**.
* Integrates with **ALB/NLB** for load balancing.
* Uses **Auto Scaling** for failover and traffic spikes.

