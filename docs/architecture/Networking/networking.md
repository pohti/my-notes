# Networking

## 🧠 **DNS & Route 53**

### **Types of DNS Servers**

> “In DNS resolution, there are four main types of domain name servers that work together to translate domain names into IP addresses.”

1. **Recursive Resolver (DNS Resolver)**

   * Provided by ISP or public DNS (e.g., Google 8.8.8.8).
   * Handles the full lookup process and caches results for faster future lookups.

2. **Root Name Server**

   * The first contact point in DNS resolution.
   * Directs the resolver to the correct TLD server (e.g., `.com`, `.org`).

3. **TLD Name Server**

   * Handles domains for a specific top-level domain.
   * Points to the authoritative name server for that domain.

4. **Authoritative Name Server**

   * Final source of truth.
   * Stores the actual DNS records (A, CNAME, MX, etc.) and responds with the IP.

✅ **Summary:**
Recursive Resolver → Root Server → TLD Server → Authoritative Server
AWS **Route 53** acts as the authoritative DNS service in this chain.

---

### **A Record vs CNAME Record**

* **A Record:** Maps a domain → IPv4 address (e.g., `example.com → 192.0.2.1`).
  Used for direct domain-to-IP mapping.

* **CNAME Record:** Maps a domain → another domain (e.g., `www.example.com → example.com`).
  Used for aliases, easier management if IPs change.

✅ **Summary:**
A Record → IP address
CNAME → Another domain name

---

### **What is Route 53?**

> “Amazon Route 53 is a scalable and highly available DNS web service that routes user requests to AWS or external resources.”

**Routing Policies in Route 53:**

* **Simple routing:** One endpoint.
* **Weighted routing:** Split traffic by weight (e.g., A/B testing).
* **Latency-based:** Route to lowest-latency region.
* **Geolocation:** Route based on user location.
* **Failover:** Switch to healthy endpoint on failure.

✅ **Benefits:**
Improves performance, availability, and fault tolerance.

---

## 🌐 **VPC & Networking Basics**

### **Basic Components of a VPC**

A **Virtual Private Cloud (VPC)** is an isolated virtual network in AWS.

| Component                              | Description                                         |
| -------------------------------------- | --------------------------------------------------- |
| **Subnets**                            | Divide the VPC network (public/private).            |
| **Internet Gateway (IGW)**             | Enables internet access for public subnets.         |
| **NAT Gateway / Instance**             | Enables outbound-only internet for private subnets. |
| **Route Tables**                       | Define how traffic moves within the VPC.            |
| **Security Groups**                    | Instance-level firewalls (stateful).                |
| **Network ACLs (NACLs)**               | Subnet-level firewalls (stateless).                 |
| **VPC Peering / VPN / Direct Connect** | Connects VPCs or on-premises networks.              |

---

### **Public vs Private Subnets**

| **Public Subnet**                         | **Private Subnet**                           |
| ----------------------------------------- | -------------------------------------------- |
| Has route to Internet Gateway.            | No direct route to Internet Gateway.         |
| Direct inbound/outbound internet access.  | No inbound access; outbound via NAT Gateway. |
| For public resources (e.g., web servers). | For private resources (e.g., DBs, apps).     |

✅ **Summary:**
Public = IGW, Private = NAT for outbound only.

---

### **NACLs vs Security Groups**

| Feature      | **Security Group**                             | **Network ACL (NACL)**                        |
| ------------ | ---------------------------------------------- | --------------------------------------------- |
| **Level**    | Instance-level                                 | Subnet-level                                  |
| **Type**     | Stateful                                       | Stateless                                     |
| **Rules**    | Allow only                                     | Allow + Deny                                  |
| **Use case** | Fine-grained control (e.g., allow SSH from IP) | Broad subnet filtering (e.g., block IP range) |

✅ **Summary:**
Security Group → Instance-level, Stateful
NACL → Subnet-level, Stateless

---

### **Are Security Groups Only for EC2?**

No — Security Groups apply to multiple AWS services:

* **ECS / EKS:** Attached to ENIs for tasks or pods.
* **Lambda (in a VPC):** Gets ENIs and uses SGs to control traffic.
* **RDS / ElastiCache:** Uses SGs for inbound DB access control.

✅ **Example:**

* ECS task SG → allows traffic only from ALB SG.
* Lambda SG → allows outbound traffic to RDS SG.

---

### **Connecting On-Premises to AWS**

Two main options:

1. **AWS Site-to-Site VPN**

   * Encrypted IPSec tunnels over the internet.
   * Quick, cost-effective, moderate performance.

2. **AWS Direct Connect**

   * Dedicated, private fiber link.
   * High bandwidth, low latency, more secure.

✅ **Integration:**
Both connect to a **Virtual Private Gateway** in your VPC.
Control routing and access via route tables, SGs, and NACLs.

---

### **NAT Gateway vs Internet Gateway**

| **Feature**   | **Internet Gateway (IGW)**                      | **NAT Gateway**                                         |
| ------------- | ----------------------------------------------- | ------------------------------------------------------- |
| **Purpose**   | Allows public inbound/outbound internet access. | Allows private instances outbound-only internet access. |
| **Used in**   | Public subnets                                  | Private subnets                                         |
| **Direction** | Two-way (inbound + outbound)                    | One-way (outbound only)                                 |

✅ **Summary:**
IGW = two-way internet access
NAT = outbound-only access for private instances

---

### **Transit Gateway**

> “A Transit Gateway acts as a central hub for connecting multiple VPCs, on-prem networks, and accounts.”

**Use it when:**

* You have **many VPCs** across accounts or regions.
* You need **centralized routing** and **simpler management**.

✅ **Example:**
Instead of 10 VPCs peered to each other (mesh), connect all to a Transit Gateway (hub-and-spoke).

---

## 🔐 **Network Security**

### **TLS vs SSL**

| **Protocol** | **Description**                                                                     |
| ------------ | ----------------------------------------------------------------------------------- |
| **SSL**      | Older protocol, deprecated (less secure).                                           |
| **TLS**      | Modern replacement for SSL, ensures confidentiality, integrity, and authentication. |

✅ **Used by:**
`HTTPS`, `CloudFront`, `ALB`, `API Gateway`, managed via **AWS Certificate Manager (ACM)**.

---

### **How TLS Works**

1. **Handshake:**

   * Client → sends supported ciphers + random number.
   * Server → responds with certificate + random number.
   * Client verifies certificate → sends encrypted pre-master key.
   * Both derive shared symmetric key.

2. **Secure Communication:**

   * Data is encrypted with symmetric encryption (e.g., AES).

3. **Integrity & Authentication:**

   * MAC ensures data not altered; certificate confirms server identity.

✅ **Summary:**
TLS uses asymmetric encryption for handshake → symmetric for data transfer.
Ensures **confidentiality, integrity, authenticity**.

---

### **Stateful vs Stateless Firewalls**

| **Type**      | **Example**     | **Behavior**                                                    |
| ------------- | --------------- | --------------------------------------------------------------- |
| **Stateful**  | Security Groups | Remembers connections; allows return traffic automatically.     |
| **Stateless** | Network ACLs    | No memory of state; requires explicit inbound & outbound rules. |

✅ **Summary:**
Stateful = Simpler, instance-level (SGs)
Stateless = Explicit rules, subnet-level (NACLs)

---

## 🧩 **OSI Model (7 Layers)**

| **Layer** | **Name**     | **Purpose / Example**                  |
| --------- | ------------ | -------------------------------------- |
| **7**     | Application  | End-user protocols (HTTP, SMTP, DNS)   |
| **6**     | Presentation | Data formatting, encryption (TLS, SSL) |
| **5**     | Session      | Connection management, authentication  |
| **4**     | Transport    | Reliable delivery (TCP, UDP)           |
| **3**     | Network      | Routing packets (IP, routers)          |
| **2**     | Data Link    | Node-to-node transfer (Ethernet, MAC)  |
| **1**     | Physical     | Transmission medium (cables, radio)    |

✅ **Mnemonic:**
**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.