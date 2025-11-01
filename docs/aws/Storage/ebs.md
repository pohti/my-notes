# EBS

## 🧩 **Overview**

* **Amazon EBS (Elastic Block Store)** provides **block-level storage volumes** for EC2 instances.
* Think of it as a **virtual hard drive** you can attach to an instance.
* Volumes persist independently from the instance’s lifecycle.

🧠 **Analogy:**
If **S3** is like a USB drive (object storage for files),
then **EBS** is like an SSD or HDD (block storage for operating systems and databases).

---

## 🧱 **Core Concepts**

| Concept              | Description                                                           |
| -------------------- | --------------------------------------------------------------------- |
| 💿 **EBS Volume**    | A virtual disk attached to an EC2 instance. Stores data persistently. |
| ⚙️ **Block Storage** | Data is stored in fixed-size blocks (unlike objects in S3).           |
| 🧷 **Snapshot**      | A point-in-time backup of a volume, stored in S3.                     |
| 🔁 **IOPS**          | Input/Output operations per second — key performance metric.          |
| 🧮 **Throughput**    | Amount of data transferred per second (MB/s).                         |
| 🏷️ **Encryption**   | EBS volumes can be encrypted using AWS KMS keys.                      |

---

## 🪟 **EBS vs Instance Store**

| Feature          | **EBS**                                       | **Instance Store**                     |
| ---------------- | --------------------------------------------- | -------------------------------------- |
| Persistence      | ✅ Data persists after instance stop/terminate | ❌ Data lost when instance stops        |
| Snapshot Support | ✅ Yes                                         | ❌ No                                   |
| Resize           | ✅ Can increase volume size                    | ❌ Fixed size                           |
| Performance      | Very high (depends on volume type)            | Very high (temporary storage)          |
| Use Case         | OS, databases, persistent storage             | Caching, scratch data, temporary files |

---

## 🧩 **Volume Lifecycle**

1. **Create Volume** → in the same Availability Zone as the EC2 instance.
2. **Attach Volume** → to the EC2 instance (like plugging in a disk).
3. **Mount & Format** → inside the instance (e.g. `/dev/xvdf`).
4. **Detach Volume** → before deleting or attaching elsewhere.
5. **Create Snapshot** → backup before resizing or migration.

---

## ⚡ **Volume Types**

| Type                                                  | Description                                               | Performance        | Use Case                            |
| ----------------------------------------------------- | --------------------------------------------------------- | ------------------ | ----------------------------------- |
| 🧊 **gp3 (General Purpose SSD)**                      | Balanced price/performance, customizable IOPS/throughput. | Up to 16,000 IOPS  | Default for most workloads          |
| ⚙️ **gp2 (General Purpose SSD)**                      | Legacy type, IOPS scales with size.                       | 3 IOPS per GB      | General workloads                   |
| 🚀 **io2 / io2 Block Express (Provisioned IOPS SSD)** | Highest durability & performance.                         | Up to 256,000 IOPS | Databases (Oracle, SQL, SAP)        |
| 🪶 **st1 (Throughput Optimized HDD)**                 | Low-cost, high throughput HDD.                            | 500 MB/s max       | Big data, log processing            |
| 📜 **sc1 (Cold HDD)**                                 | Cheapest option, lowest performance.                      | 250 MB/s max       | Archive, infrequently accessed data |

---

## 🧮 **Performance Metrics**

| Metric                                | Meaning                            | Notes                      |
| ------------------------------------- | ---------------------------------- | -------------------------- |
| ⚙️ **IOPS (Input/Output per second)** | How many read/write ops per second | Key metric for SSD volumes |
| 🚚 **Throughput (MB/s)**              | Data transfer speed                | Important for HDD volumes  |
| 🕓 **Latency**                        | Time to complete a single I/O      | Lower = better performance |

🧠 Tip:

* For **databases**, prioritize **IOPS** (e.g., `io2`).
* For **data analytics**, prioritize **throughput** (e.g., `st1`).

---

## 🔐 **EBS Encryption**

* Managed by **AWS KMS**.
* When enabled:

  * Data at rest is encrypted.
  * Snapshots and new volumes from them are automatically encrypted.
  * Data in transit between EC2 and EBS is encrypted.
* You can choose:

  * AWS-managed key (`aws/ebs`)
  * Customer-managed KMS key.

🧠 **Important:**
Encryption is applied **at volume creation** (cannot be turned on/off later — must recreate volume).

---

## 🧷 **EBS Snapshots**

| Concept                  | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| 📸 **Snapshot**          | Incremental backup stored in S3.                             |
| 🔄 **Incremental**       | Only changed blocks are saved after the first full snapshot. |
| 🧱 **Restoration**       | You can create a new volume from any snapshot.               |
| 🌍 **Cross-Region Copy** | Snapshots can be copied across regions or accounts.          |

🧠 Use snapshots for:

* Backups before changes/upgrades
* Volume cloning (e.g., from dev → prod)
* Disaster recovery setups

---

## 🚀 **Performance Optimization Tips**

✅ **Use gp3** instead of gp2 → allows independent tuning of size, IOPS, throughput.
✅ **Pre-warm volumes** for large data restores (read all blocks once).
✅ **Use RAID 0 (striping)** for combining multiple EBS volumes for higher performance.
✅ **Monitor using CloudWatch** metrics:

* `VolumeReadOps`, `VolumeWriteOps`
* `VolumeReadBytes`, `VolumeWriteBytes`
* `VolumeQueueLength`

---

## 🧠 **EBS Multi-Attach**

* Available for **io1** and **io2** volumes.
* Allows a single EBS volume to be attached to **multiple EC2 instances** (within same AZ).
* Use case: clustered applications that coordinate disk writes (e.g., Oracle RAC).

---

## 🧩 **Resizing and Modifying Volumes**

* You can **change volume size, type, and IOPS** without detaching or stopping instance.
* Steps:

  1. Modify volume in the console or CLI:

     ```bash
     aws ec2 modify-volume --volume-id vol-xxxx --size 200
     ```
  2. Wait for modification to complete.
  3. Extend the file system within the instance:

     ```bash
     sudo growpart /dev/xvdf 1
     sudo resize2fs /dev/xvdf1
     ```

---

## 🧰 **CLI Examples**

**Create a new EBS volume:**

```bash
aws ec2 create-volume \
  --availability-zone eu-north-1a \
  --size 50 \
  --volume-type gp3
```

**Attach volume to EC2:**

```bash
aws ec2 attach-volume \
  --volume-id vol-1234567890abcdef \
  --instance-id i-0123456789abcdef0 \
  --device /dev/xvdf
```

**Create a snapshot:**

```bash
aws ec2 create-snapshot \
  --volume-id vol-1234567890abcdef \
  --description "Backup before upgrade"
```

---

## 💡 **Best Practices**

* Use **gp3** for general workloads (cost-effective).
* Schedule **automatic snapshots** via AWS Backup or Lambda.
* Tag volumes and snapshots for easy management.
* Keep **volumes and instances in the same AZ**.
* Use **encrypted volumes** for all production data.
* Regularly **clean up orphaned snapshots** to reduce cost.

---

## 🧭 **Quick Summary**

| Feature         | Key Point                            |
| --------------- | ------------------------------------ |
| 🔒 Encryption   | KMS-managed, transparent to user     |
| 📸 Snapshots    | Incremental, stored in S3            |
| ⚙️ Performance  | IOPS & throughput tunable on gp3     |
| 💾 Persistence  | Data survives instance stop          |
| 🌎 Availability | Single-AZ resource                   |
| 🧩 Multi-Attach | io1/io2 only                         |
| 💰 Pricing      | Pay for provisioned storage and IOPS |
