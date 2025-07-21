Here's a **comprehensive and corrected guide to Amazon S3**, combining your original notes with detailed explanations, enhancements, and structured ordering. This version is beginner-friendly, technically accurate, and includes best practices.

---

# 🌩️ Amazon S3 - Complete Notes

---

## 📦 1. S3 Storage Classes (Organized by Use Case & Cost)

| Class                    | Use Case                                            | Availability | Durability            | Retrieval Time   | Pricing (example, US East)    |
| ------------------------ | --------------------------------------------------- | ------------ | --------------------- | ---------------- | ----------------------------- |
| **Standard**             | Frequently accessed data                            | 99.99%       | 11 9s (99.999999999%) | Milliseconds     | \~\$0.023/GB/month            |
| **Intelligent-Tiering**  | Data with unknown/changing access patterns          | 99.9%-99.99% | 11 9s                 | Milliseconds     | \~\$0.023/GB + monitoring fee |
| **Standard-IA**          | Infrequently accessed, but still needs fast access  | 99.9%        | 11 9s                 | Milliseconds     | \~\$0.0125/GB + retrieval fee |
| **One Zone-IA**          | Infrequently accessed & recreatable, in a single AZ | 99.5%        | 11 9s                 | Milliseconds     | \~\$0.01/GB + retrieval fee   |
| **Glacier Instant**      | Archive with instant retrieval                      | 99.9%        | 11 9s                 | Milliseconds     | \~\$0.004/GB + request fee    |
| **Glacier Flexible**     | Long-lived archives, minutes-to-hours retrieval     | 99.99%       | 11 9s                 | Minutes to hours | \~\$0.0036/GB                 |
| **Glacier Deep Archive** | Lowest-cost storage, rarely accessed (once/year)    | 99.99%       | 11 9s                 | Up to 12 hours   | \~\$0.00099/GB                |

> 💡 Use lifecycle policies to move data across classes as it ages.

---

## 🚦 2. CORS (Cross-Origin Resource Sharing)

* **CORS** is a **browser security feature**.
* It restricts web pages from making requests to a different domain (origin) unless explicitly allowed.
* For example, a web app at `myapp.com` cannot call an API at `api.com` unless `api.com` allows it via CORS headers.
* **Only affects browser-based requests**.
* Backend services (e.g., using curl, Postman, Lambda) are **not impacted**.

✅ To enable CORS:

* Set `Access-Control-Allow-Origin` header in the S3 bucket or CloudFront.

---

## 🔐 3. Encryption in S3

### Types of Encryption:

| Type                  | Description                                           |
| --------------------- | ----------------------------------------------------- |
| Encryption at Rest    | Protects data stored in S3 using AES-256 or AWS KMS.  |
| Encryption in Transit | Secures data using SSL/TLS protocols during transfer. |
| Symmetric Encryption  | Same key is used for encryption & decryption.         |
| Asymmetric Encryption | Public-private key pair used.                         |

---

### Server-Side Encryption (SSE):

| SSE Type | Description                                      |
| -------- | ------------------------------------------------ |
| SSE-S3   | Keys managed by S3 (AES-256 encryption).         |
| SSE-KMS  | Uses AWS KMS for more control and audit logging. |
| SSE-C    | You provide and manage your encryption keys.     |

---

### Client-Side Encryption (CSE):

* You encrypt data **before uploading to S3**.
* AWS never sees plaintext or keys.

---

## 🗂️ 4. S3 Object Key & Prefixes

An object key may look like:

```
logs/2025/07/20/log1.txt
```

Here, `logs/2025/07/20/` is the **prefix**.

S3 uses **prefixes to route and scale requests internally**.

---

## 🚀 5. Performance Scaling with Prefixes

### 🔍 What it means:

* S3 auto-scales, but performance is optimized **per prefix**.
* Older limitations (\~2018) caused throttling under a single prefix. Now, S3 supports high throughput per prefix.

### 💡 Current Throughput Limits (Per Prefix):

* **3,500 PUT/COPY/POST/DELETE requests/sec**
* **5,500 GET/HEAD requests/sec**

Using **multiple prefixes increases parallelism**:

| Prefix          | GET/sec    |
| --------------- | ---------- |
| `logs/2025/07/` | 5,500      |
| `logs/2025/08/` | 5,500      |
| `logs/2025/09/` | 5,500      |
| **Total**       | **16,500** |

---

### ✅ Best Practices:

* Use **timestamp-based prefixes** (e.g., `logs/YYYY/MM/DD/`).
* Use **randomized or hashed prefixes** (e.g., `a1/`, `b2/`) for large-scale ingestion.
* Avoid dumping all data into a single prefix like `logs/`.

---

## 📥 6. Byte-Range Fetches

* Retrieve only **a portion of an object** (by byte range).
* Ideal for:

  * Resumable downloads
  * Large file access
  * Video/audio streaming
  * Parallel downloads

---

## 📊 7. S3 Storage Lens

* **Dashboard for S3 usage & cost optimization**.
* Features:

  * Analytics across all buckets/accounts.
  * Visual reports on access patterns, object age, lifecycle policies.
  * Security and performance insights.

---

## ⬆️ 8. S3 Multi-Part Upload

### Use Case:

* For files **≥100MB**, AWS recommends multi-part upload.

### Benefits:

* Splits large files into parts and uploads in **parallel**.
* Handles **network interruptions** gracefully.
* Improves upload speed and reliability.

---

## 🌐 9. S3 Transfer Acceleration

### Description:

* Speeds up uploads/downloads over **long distances or high-latency networks**.

### How:

* Uses **Amazon CloudFront’s edge locations** to route data faster.
* Ideal for users **uploading to S3 from different regions**.

---

## 🔄 10. S3 Lifecycle Policies

Automate object transitions to cheaper storage or deletion.

### Examples:

* Move to **Glacier** after 30 days.
* **Delete objects** after 90 days.

Saves cost and improves data lifecycle efficiency.

---

## ⚙️ 11. S3 Baseline Performance

* S3 automatically scales with traffic.
* Per prefix:

  * **3,500 PUT/COPY/POST/DELETE**
  * **5,500 GET/HEAD** requests/sec
* **No limit on total prefixes per bucket**.

---

## 📁 12. Bucket Organization & Prefix Optimization

* **No limit** on number of prefixes.
* Best Practice: **create logical folder structures** using prefixes.
* Example:

  ```
  /logs/year/month/day/
  /images/campaign1/
  /data/batch01/
  ```

---

## 📌 Summary Sheet

| Feature                | Key Point                                      |
| ---------------------- | ---------------------------------------------- |
| Storage Classes        | Multiple tiers for different cost/access needs |
| Encryption             | SSE-S3, SSE-KMS, SSE-C, CSE, transit, rest     |
| CORS                   | Required for cross-origin web requests         |
| Byte-Range Fetch       | Download partial data efficiently              |
| Prefixes & Performance | Optimize throughput with multiple prefixes     |
| Transfer Acceleration  | Speed uploads over distance                    |
| Multi-Part Upload      | For large, resilient, parallel uploads         |
| Lifecycle Policies     | Automate class transitions/deletion            |
| Storage Lens           | Dashboard for optimization                     |

---
