# EKS

### 🔹 What is EKS?

**Amazon Elastic Kubernetes Service (EKS)** is a **managed Kubernetes control plane** service provided by AWS.
It automates the deployment, scaling, and management of Kubernetes clusters without requiring you to run or maintain your own control plane.

**Key idea:**

> AWS runs the Kubernetes control plane (API server, etcd, scheduler, controller manager), while you manage the worker nodes.

---

## ⚙️ **Core Components**

| Component            | Description                                                                           |
| -------------------- | ------------------------------------------------------------------------------------- |
| **Control Plane**    | Managed by AWS (highly available and automatically scaled). Runs across multiple AZs. |
| **Worker Nodes**     | EC2 instances or Fargate tasks that run your containers (Pods).                       |
| **Node Groups**      | Logical group of EC2 instances managed via an Auto Scaling Group.                     |
| **Fargate Profiles** | Let you run Pods **without managing EC2 instances** (serverless).                     |
| **Cluster Endpoint** | Public or private API endpoint used to interact with the Kubernetes API.              |

---

## 🚀 **How EKS Works**

1. You create an **EKS cluster** (AWS manages the control plane).
2. You add **worker nodes** (EC2 or Fargate) that register with the cluster.
3. You **configure kubectl** using the cluster kubeconfig.
4. You **deploy workloads** using standard Kubernetes manifests (Deployments, Services, etc.).
5. AWS integrates with other services (IAM, VPC, ALB, CloudWatch, etc.) to extend functionality.

---

## 🔐 **Security & IAM**

* **IAM Roles for Service Accounts (IRSA):**
  Allows Pods to assume specific IAM roles using OIDC identity provider integration.
* **EKS Cluster Role:**
  Used by the control plane to make AWS API calls.
* **Node IAM Role:**
  Used by worker nodes to access AWS resources (e.g., pulling images from ECR).
* **Private/Public Endpoints:**
  Restrict or expose the Kubernetes API access.

---

## 🌐 **Networking**

* **CNI Plugin (Amazon VPC CNI):**
  Each Pod gets an IP address from the VPC subnet.
* **Security Groups:**
  Control inbound/outbound traffic to worker nodes or Pods.
* **Load Balancing:**

  * **Classic / NLB / ALB** can be used for Kubernetes Services.
  * **AWS Load Balancer Controller** automates ALB/NLB provisioning.
* **Ingress Controller:**
  Manages external HTTP/S traffic routing to services.

---

## ⚡ **EKS Deployment Options**

| Type                    | Description                                         |
| ----------------------- | --------------------------------------------------- |
| **Managed Node Groups** | AWS manages EC2 lifecycle (launch, update, drain).  |
| **Self-Managed Nodes**  | You control node provisioning and updates manually. |
| **AWS Fargate**         | Serverless compute for Pods (no EC2 nodes).         |

---

## 🧩 **Integration with AWS Services**

| AWS Service               | Integration                                                       |
| ------------------------- | ----------------------------------------------------------------- |
| **CloudWatch**            | Metrics and logging for Pods and cluster components.              |
| **ECR**                   | Private Docker image registry.                                    |
| **IAM**                   | Authentication and fine-grained authorization (RBAC + IAM).       |
| **Auto Scaling**          | Cluster Autoscaler for nodes, Horizontal Pod Autoscaler for Pods. |
| **Route 53**              | DNS for services via ExternalDNS.                                 |
| **Secrets Manager / SSM** | Manage application secrets.                                       |

---

## 🧰 **EKS Tooling**

* **kubectl** → CLI for Kubernetes.
* **eksctl** → CLI tool to create and manage EKS clusters easily.
* **Helm** → Package manager for Kubernetes apps.
* **AWS CLI** → Cluster management and IAM integrations.

---

## 🔄 **Scaling in EKS**

* **Horizontal Pod Autoscaler (HPA):** Scales Pods based on CPU/memory metrics.
* **Cluster Autoscaler:** Adds/removes worker nodes automatically.
* **Managed Node Groups** handle scaling seamlessly.

---

## 🧱 **High Availability**

* Control plane spread across multiple AZs.
* Nodes should be in multiple AZs within a VPC.
* You can use multiple node groups to isolate workloads (e.g., by environment or instance type).

---

## 💰 **Cost Considerations**

You pay for:

1. **EKS control plane** ($0.10/hour per cluster).
2. **Worker nodes** (EC2 or Fargate pricing).
3. **Other AWS resources** (EBS volumes, Load Balancers, etc.).

---

## 🧠 **Common Interview Questions**

| Question                                                              | Key Points to Mention                                                                    |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **What is EKS?**                                                      | Managed Kubernetes service on AWS; control plane managed by AWS.                         |
| **EKS vs ECS?**                                                       | ECS is AWS-native orchestration; EKS is Kubernetes-compatible.                           |
| **How does EKS handle scaling?**                                      | HPA for Pods, Cluster Autoscaler for nodes, managed via node groups.                     |
| **What is IRSA in EKS?**                                              | IAM Roles for Service Accounts – fine-grained AWS permission for Pods.                   |
| **How do you manage networking in EKS?**                              | Via VPC CNI plugin; each Pod gets a VPC IP; integrates with security groups.             |
| **How can you expose services externally?**                           | Using Ingress or AWS Load Balancer Controller (ALB/NLB).                                 |
| **How do you secure your EKS cluster?**                               | Use IRSA, private API endpoints, restricted RBAC, network policies, and security groups. |
| **What’s the difference between Fargate and EC2 node groups in EKS?** | Fargate = serverless Pods, EC2 node groups = more control and flexibility.               |
| **How do you monitor EKS?**                                           | CloudWatch, Prometheus, Grafana, AWS Container Insights.                                 |

---

## 🧩 **Pro Tips**

* Use **eksctl** for cluster provisioning (simplifies setup).
* Prefer **IRSA** over node IAM roles for security.
* Deploy **Cluster Autoscaler** and **AWS Load Balancer Controller** early.
* Always use **multi-AZ** for resilience.
* Use **Helm charts** for consistent deployment management.

