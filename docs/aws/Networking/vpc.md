# VPC

## 🧩 **Overview**

* **Amazon VPC (Virtual Private Cloud)** lets you define your own isolated **virtual network** inside AWS.
* You can control:

  * IP address ranges
  * Subnet placement
  * Route tables
  * Internet and VPN connectivity
  * Security settings

👉 Think of a VPC as your **own private data center** inside AWS.

---

## 🗺️ **Core Components**

| Component                      | Description                                                                                                          |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| 🏠 **VPC**                     | A virtual network dedicated to your AWS account. You define its **CIDR block** (e.g. `10.0.0.0/16`).                 |
| 📦 **Subnet**                  | A segment of your VPC’s IP range — can be **public** or **private** depending on routing.                            |
| 🚪 **Internet Gateway (IGW)**  | Enables communication between your VPC and the public internet.                                                      |
| 🔒 **Security Group (SG)**     | Acts as a **virtual firewall** for EC2 instances — controls inbound/outbound traffic.                                |
| 🧱 **Network ACL (NACL)**      | Firewall at the **subnet level**, providing stateless traffic filtering.                                             |
| 🔁 **Route Table**             | Defines where network traffic is directed — associated with subnets.                                                 |
| 🌉 **NAT Gateway**             | Allows instances in **private subnets** to access the internet (for updates, API calls, etc.) without being exposed. |
| 🔗 **VPC Peering**             | Connects two VPCs privately using AWS’s internal network.                                                            |
| 🕸️ **PrivateLink / Endpoint** | Provides private connectivity to AWS services without using the internet.                                            |
| 🌍 **Transit Gateway**         | Central hub that connects multiple VPCs and on-prem networks.                                                        |

---

## 🧮 **VPC IP Addressing**

* Each VPC is assigned a **CIDR block** (e.g., `10.0.0.0/16` → ~65,000 IPs).
* Subnets divide this range (e.g., `10.0.1.0/24`, `10.0.2.0/24`).
* IPs inside each subnet are reserved by AWS:

  ```
  10.0.1.0     Network address
  10.0.1.1     Reserved by AWS (router)
  10.0.1.2     Reserved (DNS)
  10.0.1.3     Reserved (future use)
  10.0.1.255   Broadcast (reserved)
  ```

  ⇒ You get **251 usable IPs** per /24 subnet.

---

## 🏗️ **Subnet Types**

| Type                       | Description                             | Internet Access | Example Use Case                    |
| -------------------------- | --------------------------------------- | --------------- | ----------------------------------- |
| 🌞 **Public Subnet**       | Connected to an **Internet Gateway**.   | ✅ Yes           | Web servers, load balancers         |
| 🌙 **Private Subnet**      | No direct internet route.               | ❌ No            | Databases, internal services        |
| 🔄 **Protected (via NAT)** | Access to internet via **NAT Gateway**. | Outbound only   | App servers needing updates or APIs |

🧭 **Routing Example:**

* Public subnet route table → `0.0.0.0/0 → Internet Gateway`
* Private subnet route table → `0.0.0.0/0 → NAT Gateway`

---

## 🌐 **Internet Gateway (IGW)**

* Horizontally scaled, redundant component that connects the VPC to the internet.
* Attach one **IGW per VPC**.
* Must update the **route table** to direct traffic to the IGW:

  ```
  Destination: 0.0.0.0/0
  Target: igw-xxxxxxxx
  ```

---

## 🧭 **Route Tables**

* Define **where traffic goes**.
* Each subnet must be associated with **one route table**.
* Common routes:

  | Destination   | Target   | Purpose                  |
  | ------------- | -------- | ------------------------ |
  | `10.0.0.0/16` | local    | Default route within VPC |
  | `0.0.0.0/0`   | igw-xxxx | Internet access          |
  | `0.0.0.0/0`   | nat-xxxx | Private outbound access  |

---

## 🧱 **Network ACLs (NACLs)**

* Operate at the **subnet level**.
* **Stateless:** Need separate rules for inbound and outbound traffic.
* Rules are evaluated **in order (by rule number)**.

| Property     | Description                          |
| ------------ | ------------------------------------ |
| Default NACL | Allows all inbound/outbound traffic. |
| Custom NACL  | Denies all traffic by default.       |
| Use case     | Extra layer of security beyond SGs.  |

Example Rule:

```
100 ALLOW inbound TCP 80 (HTTP)
110 ALLOW inbound TCP 443 (HTTPS)
120 DENY  inbound ALL
```

---

## 🛡️ **Security Groups (SGs)**

* Operate at the **instance level**.
* **Stateful:** Return traffic is automatically allowed.
* Can reference other SGs as sources/destinations.

| Direction | Example                                  | Description       |
| --------- | ---------------------------------------- | ----------------- |
| Inbound   | Allow TCP 22 (SSH) from `203.0.113.0/24` | Access for admins |
| Inbound   | Allow TCP 80, 443 from `0.0.0.0/0`       | Web traffic       |
| Outbound  | Allow all                                | Common default    |

🧠 **Tip:** Start with least privilege — open ports only as needed.

---

## 🔄 **NAT Gateway**

* Allows **private subnet** instances to initiate internet connections.
* Deployed in a **public subnet** and associated with an **Elastic IP**.
* Routes:

  ```
  Destination: 0.0.0.0/0
  Target: nat-xxxxxxxx
  ```
* **Managed by AWS** (scalable, redundant).
* For lower cost → use **NAT instance** (manual setup).

---

## 🕸️ **VPC Peering**

* Connects two VPCs **privately** using AWS’s internal network.
* One-to-one connection — **no transitive routing**.
* Use cases:

  * Share resources between dev and prod VPCs.
  * Access central services (e.g., logging or monitoring).

---

## 🧩 **VPC Endpoints**

* Provide **private connectivity** to AWS services without traversing the internet.

| Type                            | Description                                | Example                |
| ------------------------------- | ------------------------------------------ | ---------------------- |
| 🌉 **Interface Endpoint (ENI)** | Elastic network interface with private IP. | Access SSM, CloudWatch |
| 🧱 **Gateway Endpoint**         | Route table entry to AWS service.          | S3, DynamoDB           |

---

## 🚪 **Transit Gateway (TGW)**

* Centralized hub to connect **multiple VPCs** and **on-premises networks**.
* Simplifies complex architectures compared to many VPC peering links.
* Offers inter-region peering and route control.

---

## 🧭 **Example Architecture**

```
VPC: 10.0.0.0/16
├── Public Subnet: 10.0.1.0/24
│   ├── IGW
│   ├── EC2 Web Server
│
├── Private Subnet: 10.0.2.0/24
│   ├── NAT Gateway
│   ├── Application Server
│
└── Private Subnet: 10.0.3.0/24
    └── Database Server (no internet)
```

---

## 💡 **Best Practices**

* Use **multiple AZs** for high availability.
* Enable **VPC Flow Logs** for network troubleshooting.
* Keep **public** and **private** workloads separated.
* Apply **least privilege** rules in SGs and NACLs.
* Use **prefix lists** or **security group references** for simpler rules.

---

## 📊 **Monitoring and Troubleshooting**

* **VPC Flow Logs** → Capture IP traffic (accepted/rejected).
* **CloudWatch Logs** → Monitor metrics and alerts.
* **Reachability Analyzer** → Test network paths between resources.

