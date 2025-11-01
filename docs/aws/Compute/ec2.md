# EC2

## 1. Basics

### **Q1. What is Amazon EC2?**

**A:**
Amazon EC2 (Elastic Compute Cloud) is a web service that provides **resizable compute capacity** in the cloud. It lets you launch virtual servers (called instances) with your desired OS, CPU, memory, storage, and network configuration.

**Example:**
You can launch an EC2 instance to host a web application or run background processing jobs.

---

### **Q2. What are EC2 instance types?**

**A:**
Instance types define **CPU, memory, storage, and network capacity**. Categories include:

| Type                  | Example            | Use Case                             |
| --------------------- | ------------------ | ------------------------------------ |
| General Purpose       | `t3`, `t4g`, `m6i` | Balanced compute and memory          |
| Compute Optimized     | `c7g`, `c6i`       | High-performance compute, batch jobs |
| Memory Optimized      | `r6g`, `x2idn`     | Databases, in-memory caches          |
| Storage Optimized     | `i4i`, `d3`        | Big data, OLTP, analytics            |
| Accelerated Computing | `p4d`, `g5`        | ML training, GPU workloads           |

---

### **Q3. What are EC2 pricing models?**

**A:**

1. **On-Demand** — pay per second/hour, flexible but expensive.
2. **Reserved Instances (RI)** — commit for 1–3 years, up to 75% cheaper.
3. **Spot Instances** — bid for unused capacity, up to 90% cheaper but can be terminated.
4. **Savings Plans** — flexible commitment based on $/hour usage.
5. **Dedicated Hosts/Instances** — physically isolated for compliance or licensing.

---

### **Q4. What’s the difference between Stop, Start, and Terminate in EC2?**

| Action        | Meaning                                   | Storage Impact                  |
| ------------- | ----------------------------------------- | ------------------------------- |
| **Stop**      | Instance shuts down, data on EBS persists | ✅ EBS retained                  |
| **Start**     | Restarts the stopped instance             | ✅ EBS retained                  |
| **Terminate** | Deletes instance permanently              | ❌ EBS deleted (unless disabled) |

---

## 2. Storage

### **Q5. What are the types of storage available for EC2?**

**A:**

1. **EBS (Elastic Block Store)** – persistent block-level storage (attached like a disk).
2. **Instance Store** – temporary storage physically attached to the host (data lost on stop/terminate).
3. **EFS (Elastic File System)** – shared file storage, scalable, NFS-based.
4. **S3** – object storage, accessed via HTTP.

---

### **Q6. What is the difference between EBS and Instance Store?**

| Feature     | EBS                      | Instance Store                  |
| ----------- | ------------------------ | ------------------------------- |
| Persistence | Data persists after stop | Lost when stopped or terminated |
| Cost        | Separate billing         | Included in instance cost       |
| Use case    | Databases, logs          | Temporary cache, buffers        |

---

## 3. Networking

### **Q7. What is a VPC in EC2?**

**A:**
A **Virtual Private Cloud** is an isolated virtual network in AWS where EC2 instances run.
You control IP ranges, subnets, route tables, NAT gateways, and security groups.

---

### **Q8. What are Security Groups and NACLs?**

| Feature  | Security Group            | NACL (Network ACL)    |
| -------- | ------------------------- | --------------------- |
| Level    | Instance-level            | Subnet-level          |
| Stateful | ✅ Yes                     | ❌ No                  |
| Rules    | Allow only                | Allow & Deny          |
| Example  | Allow SSH (22), HTTP (80) | Deny outbound port 21 |

---

### **Q9. How does EC2 instance connect to the internet?**

**A:**

* Via **Internet Gateway (IGW)** if it has a **public IP** and is in a **public subnet**.
* Or via a **NAT Gateway** if it’s in a **private subnet** and needs outbound internet access.

---

## 4. Security

### **Q10. What are key pairs in EC2?**

**A:**
Key pairs are used for **SSH authentication**. AWS stores the public key, and you keep the private key to log into your instance.

---

### **Q11. How do you securely connect to an EC2 instance?**

**A:**

1. Use SSH with private key (`.pem` file).
2. Use **Session Manager** (no open ports required).
3. Restrict SSH to specific IPs in Security Groups.

---

### **Q12. What is an IAM Role in EC2?**

**A:**
An **IAM Role** is attached to an instance to grant permissions (e.g., S3 read access) **without storing AWS credentials** on the machine.

---

## 5. Monitoring & Scaling

### **Q13. How do you monitor EC2 performance?**

**A:**

* **Amazon CloudWatch**: tracks metrics (CPU, memory, disk, network).
* **CloudWatch Alarms**: trigger notifications or auto-scaling actions.

---

### **Q14. What is Auto Scaling?**

**A:**
Automatically adjusts the number of EC2 instances based on demand.
Helps maintain performance and control costs.

---

### **Q15. What is an Elastic Load Balancer (ELB)?**

**A:**
Distributes incoming traffic across multiple EC2 instances.
Types:

* Application Load Balancer (ALB) – Layer 7 (HTTP/HTTPS)
* Network Load Balancer (NLB) – Layer 4 (TCP)
* Gateway Load Balancer – for firewalls/proxies

---

## 6. Advanced Topics

### **Q16. What happens when an EC2 instance fails?**

**A:**
AWS automatically moves the instance to **healthy hardware** in the same AZ when possible.
To handle app-level recovery, use **Auto Scaling Groups** with **health checks**.

---

### **Q17. What are EC2 placement groups?**

**A:**
They control **how instances are physically placed**:

| Type          | Description                                                                 |
| ------------- | --------------------------------------------------------------------------- |
| **Cluster**   | Close together for low-latency, high throughput                             |
| **Spread**    | Spread across different hardware for fault tolerance                        |
| **Partition** | Groups across partitions (useful for large distributed systems like Hadoop) |

---

### **Q18. What’s a user data script in EC2?**

**A:**
A shell script executed **on first boot** of an instance.
Used for installing software or configuring environment automatically.

Example:

```bash
#!/bin/bash
yum update -y
yum install -y nginx
systemctl start nginx
```

---

### **Q19. How do you migrate an EC2 instance to another region?**

**A:**

1. Create an AMI (Amazon Machine Image).
2. Copy the AMI to another region.
3. Launch a new instance using that AMI.

---

### **Q20. How do you back up and restore EC2?**

**A:**

* Take **EBS snapshots** for backups.
* Restore by creating a new volume from a snapshot and attaching it to an instance.

---

## 7. Troubleshooting

### **Q21. Why can’t you SSH into an EC2 instance?**

**Possible Causes:**

* Wrong key pair or permissions on `.pem` file.
* Security group doesn’t allow port 22.
* Instance in private subnet without proper routing.
* Network ACL denies inbound traffic.
* User (`ec2-user`, `ubuntu`, etc.) incorrect.

---

### **Q22. How do you reduce EC2 costs?**

**A:**

* Use **Spot** or **Reserved Instances**.
* Enable **Auto Scaling** and terminate idle instances.
* Use **Graviton-based** instances (ARM CPUs).
* Move static files to **S3 + CloudFront**.

