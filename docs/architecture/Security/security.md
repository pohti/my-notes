# Security

## 🔐 AWS Security Overview

### Q: How do you secure an application on AWS?

**A:** Securing an AWS application follows a **defense-in-depth** strategy, covering multiple layers:

1. **Network Security**

   * Design VPCs with **public/private subnets**.
   * Use:

     * **NACLs** (stateless) for subnet-level control.
     * **Security Groups** (stateful) for instance-level control.
   * Restrict inbound/outbound traffic tightly.

2. **Identity & Access Management (IAM)**

   * Apply **least privilege** principles.
   * Use **IAM roles** instead of long-term credentials.
   * Enable **Multi-Factor Authentication (MFA)**.
   * Use **Cognito** for user authentication if needed.

3. **Data Protection**

   * Encrypt **data at rest** using **KMS-managed keys** (S3, RDS, EBS).
   * Enforce **TLS** for **data in transit**.

4. **Application & Network Protection**

   * Use **AWS WAF** and **AWS Shield** to prevent web exploits and DDoS attacks.

5. **Monitoring & Auditing**

   * Enable **CloudTrail** and **CloudWatch** for logging and anomaly detection.
   * Conduct regular **security patches** and **vulnerability assessments**.

---

## 🧩 Identity and Access Management (IAM)

### Q: What is IAM and how do you manage permissions securely?

**A:**
**IAM (Identity and Access Management)** controls who can access AWS resources and what actions they can perform.

**Best Practices:**

* Apply **least privilege** — grant only necessary permissions.
* Prefer **IAM roles** (temporary credentials) over long-term access keys.
* Enforce **MFA** for privileged users.
* Use **IAM Access Analyzer** to audit unused or overly broad permissions.
* Use **permission boundaries** and **Service Control Policies (SCPs)** for multi-account setups with **AWS Organizations**.
* Rotate and monitor credentials regularly.

---

## 🔒 Data Encryption

### Q: How do you secure data at rest and in transit?

**A:**

**Data at Rest:**

* Use AWS-managed or customer-managed **KMS keys** to encrypt data in:

  * S3, EBS, RDS, DynamoDB, etc.
* Enable **automatic key rotation** and **CloudTrail logging** for key usage.

**Data in Transit:**

* Enforce **TLS/SSL** across all communication channels.
* Use **HTTPS** via **AWS Certificate Manager (ACM)** for public APIs/websites.
* Enable **SSL/TLS** for internal communication (e.g., microservices, DB connections).

✅ **Rule of thumb:** Always encrypt **data at rest and in transit** using IAM and KMS key policies to tightly control access.

---

### Q: Explain data encryption at rest and in transit in AWS.

**A:**

* **Encryption at Rest:** Protects data stored on disks (S3, RDS, EBS, DynamoDB) using **KMS**. Prevents unauthorized reading of stored data.
* **Encryption in Transit:** Protects data moving between systems using **TLS** (HTTPS). Prevents interception or tampering during transmission.

Together, they ensure **confidentiality and integrity** of data throughout its lifecycle.

---

## 🧱 API Security

### Q: What are security best practices when exposing APIs on AWS?

**A:**

* Use **API Gateway** with **authentication/authorization** via:

  * **Cognito user pools**, or
  * **IAM roles/policies**.
* Enforce **HTTPS/TLS** for encrypted communication.
* Implement **throttling and rate limiting** to prevent abuse.
* Use **AWS WAF** for protection from **SQL injection** and **XSS**.
* Log and monitor with **CloudWatch Logs** and **CloudTrail**.
* Apply **least privilege** for IAM roles interacting with APIs.
* Rotate **API keys/tokens** and regularly review permissions.

These practices ensure APIs are **secure, resilient, and auditable**.

---

## 🧰 AWS WAF (Web Application Firewall)

### Q: What is AWS WAF and when would you use it?

**A:**
**AWS WAF** protects web applications from common web exploits such as **SQL injection**, **cross-site scripting (XSS)**, and **DDoS attacks**.

**Use Cases:**

* Protect **CloudFront**, **API Gateway**, or **Application Load Balancer** endpoints.
* Create **custom rules** to allow/block requests based on IPs, headers, or patterns.
* Combine with **AWS Shield** for enhanced DDoS protection.
* Monitor security events with **real-time metrics** and **logging**.

📌 **Use it whenever you expose a public-facing web application** to filter malicious traffic before it reaches your backend.

