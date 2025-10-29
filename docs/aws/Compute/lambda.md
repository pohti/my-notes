# Lambda

## 🔹 1. Basics

### **Q1. What is AWS Lambda?**

**A:**
AWS Lambda is a **serverless compute service** that lets you run code **without managing servers**.
You upload your code, and Lambda automatically runs it **in response to events** — scaling up or down automatically.

**Key idea:** Pay only for the compute time you use.

---

### **Q2. What are typical use cases for Lambda?**

**A:**

* Running backend logic for APIs (via API Gateway)
* Data processing (S3 file uploads, DynamoDB streams, Kinesis)
* Event-driven automation (CloudWatch events, SNS, SQS)
* Scheduled tasks (cron jobs)
* Chatbots or Slack integrations
* Infrastructure automation (IaC hooks)

---

## 🔹 2. Core Concepts

### **Q3. What are the main components of Lambda?**

| Component                 | Description                                                                    |
| ------------------------- | ------------------------------------------------------------------------------ |
| **Function**              | Your code + configuration (runtime, handler, memory, timeout, IAM role).       |
| **Event Source**          | The trigger that invokes your Lambda (API Gateway, S3, SNS, etc.).             |
| **Execution Role**        | IAM role that grants permissions for your Lambda to access other AWS services. |
| **Execution Environment** | Isolated container with your runtime and dependencies.                         |
| **Layers**                | Optional packages shared across functions (libraries, SDKs).                   |

---

### **Q4. How does Lambda handle scaling?**

**A:**
Lambda automatically scales **horizontally** by running multiple function instances in parallel.
Each function invocation runs in a separate environment.

There’s no manual scaling — AWS handles concurrency automatically (up to account limits).

---

## 🔹 3. Function Configuration

### **Q5. What are key configuration settings for a Lambda function?**

| Setting                   | Description                                                                         |
| ------------------------- | ----------------------------------------------------------------------------------- |
| **Memory**                | 128 MB – 10 GB; also affects CPU proportionally.                                    |
| **Timeout**               | Max 15 minutes (900 seconds).                                                       |
| **Environment Variables** | Store app config, secrets (use Parameter Store/Secrets Manager for sensitive data). |
| **Concurrency**           | Number of executions running simultaneously.                                        |
| **Reserved Concurrency**  | Guarantees a set number of concurrent executions for a specific function.           |

---

### **Q6. What runtimes are supported by AWS Lambda?**

**A:**

* Node.js
* Python
* Java
* Go
* .NET Core
* Ruby
* Custom runtimes (via AWS Lambda Runtime API)

---

## 🔹 4. Invocation & Triggers

### **Q7. What are the types of invocation in Lambda?**

| Type                     | Description                           | Example                        |
| ------------------------ | ------------------------------------- | ------------------------------ |
| **Synchronous**          | Caller waits for result.              | API Gateway, CLI invoke        |
| **Asynchronous**         | Queued and retried automatically.     | S3, SNS, CloudWatch Events     |
| **Event Source Mapping** | Continuous polling from data streams. | DynamoDB Streams, Kinesis, SQS |

---

### **Q8. How do you trigger a Lambda function?**

**A:**
Lambda integrates with many AWS services, e.g.:

* **API Gateway** → REST/HTTP APIs
* **S3** → file upload/delete events
* **SNS/SQS** → message events
* **CloudWatch Events / EventBridge** → scheduled or event-based triggers
* **DynamoDB Streams** → change data capture
* **Cognito** → auth triggers

---

### **Q9. How does Lambda handle retries for asynchronous invocations?**

**A:**

* Automatically retries twice on failure (total 3 attempts).
* You can configure **Dead Letter Queues (DLQ)** or **Event Destinations** to handle failed events.

---

## 🔹 5. Security

### **Q10. How does IAM work with Lambda?**

**A:**
Lambda functions use an **IAM execution role** to access AWS resources.
This role should have the **least privilege** necessary (e.g., only read/write to a specific S3 bucket).

---

### **Q11. How do you secure Lambda functions?**

**Best practices:**

* Use least-privilege IAM roles.
* Store secrets in **Secrets Manager** or **Parameter Store**.
* Use **VPC access** only if required (for private resources).
* Enable **encryption at rest and in transit**.
* Use **environment variable encryption** (KMS).

---

## 🔹 6. Networking

### **Q12. Can Lambda run inside a VPC?**

**A:**
Yes — Lambda can be attached to a VPC subnet.
This is required when accessing **private resources** (e.g., RDS without public endpoint).

When you attach a VPC:

* Lambda uses **ENIs (Elastic Network Interfaces)** to connect.
* Cold start time slightly increases.
* Must configure **NAT Gateway** for internet access (if needed).

---

## 🔹 7. Performance & Scaling

### **Q13. What are Cold Starts and Warm Starts?**

**A:**

* **Cold start:** Lambda creates a new container → initialize runtime + load code → slower (~100ms–several seconds).
* **Warm start:** Reuses an existing container → faster execution.

**To reduce cold starts:**

* Use **Provisioned Concurrency** (keeps functions warm).
* Keep function lightweight and dependencies small.
* Avoid unnecessary VPC attachments.

---

### **Q14. What is Provisioned Concurrency?**

**A:**
A Lambda feature that **pre-warms containers** so they’re always ready to respond instantly.
Useful for latency-sensitive apps (APIs, chatbots, etc.).

---

## 🔹 8. Monitoring & Logging

### **Q15. How do you monitor AWS Lambda?**

**A:**

* **CloudWatch Metrics** (invocations, duration, errors, throttles).
* **CloudWatch Logs** (stdout/stderr output).
* **X-Ray** (traces and performance insights).
* **AWS Lambda Insights** (CPU/memory usage).

---

### **Q16. What are common Lambda CloudWatch metrics?**

| Metric                   | Description                                               |
| ------------------------ | --------------------------------------------------------- |
| **Invocations**          | Number of times function is called.                       |
| **Duration**             | Execution time in milliseconds.                           |
| **Errors**               | Count of failed executions.                               |
| **Throttles**            | Number of executions throttled due to concurrency limits. |
| **ConcurrentExecutions** | Number of functions running at once.                      |

---

## 🔹 9. Development & Deployment

### **Q17. How can you deploy Lambda functions?**

**A:**

* Manually via AWS Console.
* **AWS CLI** (`aws lambda update-function-code`).
* **AWS SAM (Serverless Application Model)** — declarative IaC for serverless.
* **AWS CDK** — programmatic IaC using Python/TypeScript.
* **Terraform** — IaC for multi-cloud.
* **CI/CD** pipelines via CodePipeline, CodeDeploy, or GitHub Actions.

---

### **Q18. What is a Lambda Layer?**

**A:**
A **Lambda Layer** is a package of libraries or dependencies shared across multiple functions.
Reduces code duplication and simplifies updates.

Example:
A shared layer for Python dependencies like `requests` or `boto3`.

---

### **Q19. How do you version Lambda functions?**

**A:**

* Every deployment creates a **new version**.
* Versions are immutable.
* You can create **aliases** (e.g., “prod”, “staging”) that point to specific versions.

---

### **Q20. What’s the difference between versions and aliases in Lambda?**

| Concept     | Description                                                                 |
| ----------- | --------------------------------------------------------------------------- |
| **Version** | Immutable snapshot of code + configuration.                                 |
| **Alias**   | Pointer to a version (used for traffic shifting or blue/green deployments). |

---

## 🔹 10. Integration & Event Patterns

### **Q21. How does Lambda integrate with API Gateway?**

**A:**
Lambda can serve as the **backend for REST or HTTP APIs**.
API Gateway routes HTTP requests → invokes Lambda → returns HTTP response.

You can define mappings for request/response transformation and authentication (Cognito, IAM).

---

### **Q22. How can Lambda integrate with S3?**

**A:**
You can trigger a Lambda when:

* An object is uploaded or deleted in S3.
* Common for generating thumbnails, data ingestion, or log parsing.

---

### **Q23. How does Lambda work with SQS and SNS?**

| Service | Behavior                                               |
| ------- | ------------------------------------------------------ |
| **SQS** | Lambda polls the queue, processes messages in batches. |
| **SNS** | SNS pushes events directly to Lambda.                  |

---

## 🔹 11. Error Handling & Retries

### **Q24. How do you handle Lambda errors?**

**A:**

* Use **try/catch** in your code.
* Enable **DLQs (Dead Letter Queues)** for async invocations.
* Use **Event Destinations** for success/failure events.
* Monitor CloudWatch error metrics.

---

### **Q25. What is a Dead Letter Queue (DLQ)?**

**A:**
A **DLQ** (SQS or SNS) stores events that failed processing after retries.
You can reprocess or analyze failed events later.

---

## 🔹 12. Cost Optimization

### **Q26. How is Lambda priced?**

**A:**
You pay for:

* **Invocations**: $0.20 per million requests (first 1M free).
* **Compute time**: based on GB-seconds (memory × duration).
* **Provisioned Concurrency**: additional hourly charge.

---

### **Q27. How do you reduce Lambda costs?**

**A:**

* Use **smaller memory** if CPU not bottlenecked.
* Keep functions short-lived.
* Avoid unnecessary invocations.
* Reuse connections (e.g., DB clients) across invocations.
* Use **AWS Step Functions** to coordinate multiple Lambdas efficiently.

---

## 🔹 13. Troubleshooting

### **Q28. Why might your Lambda be timing out?**

**Possible Causes:**

* Timeout value too low.
* External API or database too slow.
* Function in VPC without proper internet/NAT access.
* Infinite loops or unhandled async logic.

---

### **Q29. What causes throttling in Lambda?**

**A:**
When the function exceeds its **concurrency limit**.
AWS either:

* Queues requests (async), or
* Returns throttling errors (sync).

---

### **Q30. How do you debug Lambda locally?**

**A:**

* Use **AWS SAM CLI** (`sam local invoke` or `sam local start-api`).
* Use **Docker** to emulate runtime.
* Integrate with **VSCode Debugger** or **AWS Toolkit** extensions.

---

## 🔹 14. Real-world Architecture Questions

### **Q31. How would you use Lambda for a web application?**

**A:**

* API Gateway → Lambda → DynamoDB / RDS
* Store static assets in S3 + CloudFront
* Use Cognito for authentication
* Use CloudWatch for monitoring

---

### **Q32. When *not* to use Lambda?**

**A:**

* Long-running or CPU-intensive tasks (>15 min).
* Large dependencies or containerized workloads.
* Low-latency applications that require predictable cold starts.
* Applications needing constant network connections (e.g., WebSockets).
