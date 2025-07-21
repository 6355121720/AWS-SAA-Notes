Your handwritten notes are already excellent and cover most key points across **all EBS volume types**. I’ll now enhance your notes with:

1. **Correct formatting**
2. **Any missed advanced features**
3. **Clear, deep technical insights**
4. **Edge use cases and default behaviors**
5. **Block Express for `io2`**
6. **Comparison and recommendation table**

---

# 📘 Finalized Notes: **Amazon EBS Volume Types (In Depth)**

Amazon EBS offers multiple **volume types**, categorized into:

### 🔹 SSD-backed (Low latency, IOPS-focused)

* **gp2** – General Purpose SSD (legacy, burst model)
* **gp3** – General Purpose SSD (modern, configurable)
* **io1** – Provisioned IOPS SSD (gen 1)
* **io2** – Provisioned IOPS SSD (gen 2)
* **io2 Block Express** – High-end NVMe SSD volumes (newer tier)

### 🔸 HDD-backed (Low-cost, throughput-focused)

* **st1** – Throughput Optimized HDD
* **sc1** – Cold HDD

---

## 🔹 1. **General Purpose SSD (gp2 and gp3)**

### 🧾 gp2 (Burst-based SSD) – \[Legacy]

| Feature     | Detail                                              |
| ----------- | --------------------------------------------------- |
| IOPS        | 3 IOPS/GB, up to 16,000 IOPS                        |
| Burst IOPS  | Up to **3000** IOPS (if credits available)          |
| Throughput  | Up to 250 MB/s                                      |
| Volume Size | 1 GiB – 16 TiB                                      |
| Use Case    | Unpredictable workloads: small DBs, boot volumes    |
| Limitation  | IOPS tied to size (e.g. 100 GB = 300 IOPS baseline) |

✅ **Note:** Good for variable workloads, but **credit-based**, so not suitable for sustained high IOPS.

---

### 🧾 gp3 (Modern SSD) – \[Recommended Default]

| Feature             | Detail                                                |
| ------------------- | ----------------------------------------------------- |
| IOPS                | Default 3000, up to 16,000 IOPS (independent of size) |
| Throughput          | Up to 1000 MB/s                                       |
| IOPS and throughput | **Fully configurable separately**                     |
| Use Case            | Web servers, low-latency apps                         |
| Benefit             | No burst model; consistent performance                |
| Cost                | Cheaper and more predictable than gp2                 |

✅ **Tip:** Always prefer **gp3 over gp2** unless legacy compatibility needed.

---

## 🔹 2. **Provisioned IOPS SSD (io1 and io2)**

### 🧾 io1

| Feature        | Detail                                    |
| -------------- | ----------------------------------------- |
| IOPS           | Up to 64,000 (Nitro EC2)                  |
| Throughput     | Up to 1000 MB/s                           |
| Durability     | 99.8% – 99.9%                             |
| IOPS min ratio | 0.5 IOPS/GB                               |
| Use Case       | High-performance DBs, low latency apps    |
| Limitation     | Higher cost and lower durability than io2 |

---

### 🧾 io2 (Next Gen Provisioned SSD)

| Feature         | Detail                               |
| --------------- | ------------------------------------ |
| IOPS            | Same as io1: 64,000                  |
| Throughput      | 1000 MB/s                            |
| Durability      | **99.999%** (100× better than io1)   |
| IOPS min ratio  | **1 IOPS/GB**                        |
| Elastic Volumes | ✅ Supported                          |
| Use Case        | Mission-critical DBs, SAP HANA, etc. |

✅ **Tip:** io2 is better in all aspects compared to io1 and often costs the same or less.

---

### 🧾 io2 Block Express (Enterprise NVMe SSD)

| Feature        | Detail                                     |
| -------------- | ------------------------------------------ |
| Max IOPS       | **256,000**                                |
| Max Throughput | **4000 MB/s**                              |
| Latency        | Single-digit **microseconds**              |
| Interface      | **NVMe over PCIe**                         |
| Use Case       | Enterprise-scale DBs, SAP HANA, Oracle RAC |
| Requirements   | Nitro-based EC2, limited instance types    |

✅ Ultra-high-performance storage for largest production systems.

---

## 🔸 3. **Throughput Optimized HDD (st1)**

| Feature             | Detail                                    |
| ------------------- | ----------------------------------------- |
| Baseline Throughput | 40 MB/s per TB                            |
| Burst Throughput    | Up to **500 MB/s**                        |
| IOPS                | Scales with throughput (\~500 IOPS max)   |
| Volume Size         | 125 GiB – 16 TiB                          |
| Use Case            | Streaming logs, big data, data warehouses |
| Cost                | Low (but higher than `sc1`)               |

✅ Optimized for **large, sequential** read/write operations.

---

## 🔸 4. **Cold HDD (sc1)**

| Feature             | Detail                               |
| ------------------- | ------------------------------------ |
| Baseline Throughput | 12 MB/s per TB                       |
| Burst Throughput    | Up to **250 MB/s**                   |
| IOPS                | Lower than `st1` (\~250 IOPS max)    |
| Volume Size         | 125 GiB – 16 TiB                     |
| Use Case            | Archival, infrequent access, backups |
| Cost                | **Lowest among all EBS types**       |

✅ Best for **cold storage** use cases that rarely need access.

---

## 📊 Final Comparison Table:

| Volume Type       | Max IOPS               | Max Throughput | Performance Model | Cost               | Ideal Use                  |
| ----------------- | ---------------------- | -------------- | ----------------- | ------------------ | -------------------------- |
| gp2               | 16,000 (burst to 3000) | 250 MB/s       | Burst-based       | Low                | Boot, web app, dev/test    |
| gp3               | 16,000                 | 1000 MB/s      | Configurable      | Lower than gp2     | All general-purpose        |
| io1               | 64,000                 | 1000 MB/s      | Fixed IOPS        | High               | Legacy I/O-heavy workloads |
| io2               | 64,000                 | 1000 MB/s      | Fixed IOPS        | Same/less than io1 | Critical DBs               |
| io2 Block Express | 256,000                | 4000 MB/s      | Fixed + NVMe      | Premium            | SAP HANA, large-scale      |
| st1               | \~500                  | 500 MB/s       | Burst throughput  | Low                | Big data, logs             |
| sc1               | \~250                  | 250 MB/s       | Burst throughput  | **Lowest**         | Cold archive, backup       |

---

## 🧠 Pro Tips:

* ✅ **Use `gp3`** for almost all general-purpose workloads.
* ✅ **Use `io2`** or `io2 Block Express` for critical DBs and ultra-low latency.
* ✅ **Use `st1`** for cost-effective, high-throughput sequential access.
* ✅ **Use `sc1`** only for archive or very cold data.

---

Let me know if you want this exported as a **PDF study sheet** or a **one-page printable cheat sheet**!
