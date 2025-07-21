Here’s **AWS CloudFront** explained **in-depth yet short**, perfect for your notes:

---

## ☁️ AWS CloudFront — In Depth, In Short

### 🔹 What is it?

**CloudFront = AWS’s global CDN (Content Delivery Network)**
It **caches and delivers content (static/dynamic, APIs, video, etc.)** with low latency via **Edge Locations** near users.

---

### 🔹 Core Components

| Component                                 | Description                                                                 |
| ----------------------------------------- | --------------------------------------------------------------------------- |
| **Edge Location**                         | Globally distributed caching servers (POPs) near users                      |
| **Origin**                                | Your source server (e.g., S3, EC2, ALB, API Gateway, custom HTTP server)    |
| **Distribution**                          | Config for how CloudFront serves content (can be web or RTMP)               |
| **Cache Behavior**                        | Rules for how different URL paths are handled (cache policy, headers, etc.) |
| **Origin Access Control (OAC)** / **OAI** | Secure access to private S3 buckets                                         |

---

### 🔹 How it Works (Flow)

1. User requests a file (e.g., image, HTML, API call).
2. CloudFront checks **Edge cache**.

   * If **hit**, returns cached content.
   * If **miss**, forwards to **origin**, caches response.
3. Returns content to user via nearest edge → **low latency**.

---

### 🔹 Features

| Feature                               | Description                                             |
| ------------------------------------- | ------------------------------------------------------- |
| ✅ Global Edge Network                 | 450+ POPs worldwide for low-latency delivery            |
| ✅ Caching                             | TTL-based content caching + customizable cache policies |
| ✅ Dynamic + Static Content            | Supports both (e.g., APIs, HTML, JS, video, etc.)       |
| ✅ TLS + HTTPS                         | Full encryption, custom SSL supported                   |
| ✅ Geo/Device Restrictions             | Restrict access based on country or device              |
| ✅ WAF Integration                     | Built-in AWS WAF protection                             |
| ✅ Lambda\@Edge / CloudFront Functions | Run custom code at the edge (modify req/res)            |
| ✅ Signed URLs/Cookies                 | Restrict access to premium/private content              |
| ✅ Origin Failover                     | Set secondary origin if primary fails                   |
| ✅ Real-time Logging                   | Detailed access logs + viewer request logs              |
| ✅ Cost Optimization                   | Regional pricing, cache hit ratio improves cost         |

---

### 🔹 Use Cases

* ✅ Website acceleration (HTML, CSS, JS)
* ✅ Global delivery of images, videos, media
* ✅ API Gateway frontend caching
* ✅ Restrict S3 file access (OAI/OAC)
* ✅ Protect apps with AWS WAF + Lambda\@Edge

---

Let me know if you want this added to `cloudfront.md`, or need **comparison with S3 Static Hosting + Route 53**.
