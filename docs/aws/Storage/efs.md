# EFS

## Overview

* **Amazon Elastic File System (EFS)** provides **scalable, fully managed, shared file storage** for use with AWS compute services (e.g., EC2, ECS, Lambda).
* It uses the **NFS (Network File System)** protocol, allowing multiple instances to access the same file system concurrently.
* EFS automatically grows and shrinks as you add or remove files, eliminating the need to provision capacity.

---

## Key Features

| Feature           | Description                                                    |
| ----------------- | -------------------------------------------------------------- |
| Fully Managed     | AWS handles infrastructure, scaling, and maintenance.          |
| Elastic Scaling   | Automatically scales up or down with your data usage.          |
| Shared Access     | Multiple EC2 instances can read and write simultaneously.      |
| NFS Protocol      | Supports NFSv4 and NFSv4.1 for Linux-based systems.            |
| High Availability | Data is stored redundantly across multiple Availability Zones. |
| Regional Service  | Each file system is available across all AZs in a region.      |
| Integration       | Works with EC2, ECS, EKS, and AWS Lambda.                      |

---

## EFS vs EBS vs S3

| Feature           | EFS                           | EBS                 | S3                       |
| ----------------- | ----------------------------- | ------------------- | ------------------------ |
| Storage Type      | File storage                  | Block storage       | Object storage           |
| Access Type       | Shared (multi-instance)       | Single-instance     | Web-scale (HTTP API)     |
| Protocol          | NFS                           | Block-level         | REST/HTTPS               |
| Scalability       | Automatic                     | Manual              | Automatic                |
| Persistence       | Persistent                    | Persistent          | Persistent               |
| Performance       | High throughput, shared       | Low-latency block   | Depends on storage class |
| Typical Use Cases | Web servers, CMS, shared apps | Databases, OS disks | Backups, static assets   |

---

## Architecture

EFS consists of:

1. **File System** – The top-level resource that stores data.
2. **Mount Targets** – Network endpoints in each Availability Zone that allow instances to connect.
3. **Security Groups** – Control inbound/outbound access to mount targets.
4. **Access Points** – Application-specific entry points with different permissions and paths.

### Access Pattern

* EC2 instances connect using the **NFS mount command**.
* Data is automatically replicated across multiple AZs for durability.

---

## Performance Modes

| Mode            | Description                                                     | Use Case                               |
| --------------- | --------------------------------------------------------------- | -------------------------------------- |
| General Purpose | Low latency, suitable for most workloads.                       | Web servers, CMS, home directories.    |
| Max I/O         | Higher throughput and concurrency with slightly higher latency. | Big data, analytics, media processing. |

---

## Throughput Modes

| Mode        | Description                                                               | Use Case                                    |
| ----------- | ------------------------------------------------------------------------- | ------------------------------------------- |
| Bursting    | Scales throughput with storage size. Small systems can burst temporarily. | Default for general workloads.              |
| Provisioned | Fixed throughput independent of storage size.                             | Consistent performance for heavy workloads. |

---

## Storage Classes

| Class                      | Description                                                        | Use Case                                               |
| -------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------ |
| Standard                   | Frequently accessed files, stored redundantly across multiple AZs. | Primary data, applications, shared content.            |
| Infrequent Access (EFS-IA) | Lower cost, slightly higher access latency.                        | Backups, old project data, less frequently used files. |

**Lifecycle Management:**
You can configure automatic movement of files from **Standard** to **Infrequent Access** after a set period of inactivity (e.g., 30 days).

---

## Mounting EFS

### Prerequisites

* EC2 instance in the **same VPC** as the EFS mount target.
* NFS utilities installed (e.g., `amazon-efs-utils` or `nfs-utils`).
* Security group rules that allow NFS traffic (TCP 2049).

### Example Mount Commands

Using EFS mount helper:

```bash
sudo yum install -y amazon-efs-utils
sudo mkdir /mnt/efs
sudo mount -t efs fs-12345678:/ /mnt/efs
```

Using NFS client:

```bash
sudo yum install -y nfs-utils
sudo mkdir /mnt/efs
sudo mount -t nfs4 -o nfsvers=4.1 fs-12345678.efs.eu-north-1.amazonaws.com:/ /mnt/efs
```

Persistent mount (add to `/etc/fstab`):

```
fs-12345678:/ /mnt/efs efs defaults,_netdev 0 0
```

---

## Security

1. **Network Security**

   * Controlled via **VPC**, **subnets**, and **security groups**.
   * Ensure port **2049 (NFS)** is open between EC2 and EFS mount targets.

2. **Access Control**

   * **POSIX permissions** (user, group, others).
   * **Access Points** for application-level isolation with custom paths and permissions.

3. **Encryption**

   * **At rest:** Uses AWS KMS-managed keys.
   * **In transit:** TLS encryption supported (using mount helper with `-o tls`).

Example:

```bash
sudo mount -t efs -o tls fs-12345678:/ /mnt/efs
```

---

## Backup and Durability

* Files are automatically replicated across multiple AZs.
* Supports **AWS Backup** for automated scheduled backups.
* You can create on-demand backups or restore from previous ones.

---

## Monitoring and Metrics

You can monitor EFS using **Amazon CloudWatch**. Common metrics:

* `BurstCreditBalance`
* `PercentIOLimit`
* `ClientConnections`
* `DataReadIOBytes` and `DataWriteIOBytes`

Alarms can be configured for:

* High usage
* Latency issues
* Low burst credits

---

## Common Use Cases

| Category               | Examples                            |
| ---------------------- | ----------------------------------- |
| Shared file storage    | Web servers, WordPress, CMS         |
| Container storage      | ECS, EKS persistent storage         |
| Data processing        | Big data and analytics pipelines    |
| Developer environments | Shared project directories          |
| Machine learning       | Shared datasets for training models |

---

## Cost Considerations

EFS pricing is based on:

1. **Storage used (per GB/month)** — Standard or IA class.
2. **Throughput mode** (provisioned or bursting).
3. **Requests** (for IA access).
4. **Data transfer** within the same region is free; cross-region access not supported.

**Optimization Tips:**

* Use Lifecycle Management to move infrequently accessed files to EFS-IA.
* Use access patterns to select correct performance and throughput modes.
* Delete unused file systems and mount targets to reduce costs.

---

## Best Practices

* Use **General Purpose performance mode** for most workloads.
* Use **EFS-IA** for rarely accessed files.
* Mount with **TLS encryption** for sensitive data.
* Tag your file systems for cost tracking and management.
* Set up **CloudWatch alarms** to monitor usage and performance.
* Use **Access Points** to isolate app-level file access.
* Integrate **AWS Backup** for compliance and disaster recovery.

---

## Summary

| Property     | Description                                    |
| ------------ | ---------------------------------------------- |
| Storage Type | Shared file system                             |
| Protocol     | NFSv4/v4.1                                     |
| Access       | Multi-instance concurrent access               |
| Scaling      | Automatic                                      |
| Availability | Multi-AZ, regional                             |
| Encryption   | At rest and in transit                         |
| Backup       | AWS Backup integration                         |
| Use Cases    | Web servers, container workloads, data sharing |

