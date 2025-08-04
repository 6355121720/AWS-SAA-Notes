Here’s a refined and deeply structured version of your AWS Certificate Manager (ACM) notes, ideal for long-term revision:

---

# 📜 AWS Certificate Manager (ACM) – In-Depth Notes

## 🔐 Purpose

**AWS Certificate Manager (ACM)** helps you:

* Provision
* Manage
* Deploy **TLS certificates**
  for **HTTPS** encryption (in-flight data protection).

TLS (formerly SSL) ensures secure communication between clients and servers. ACM automates much of the pain around managing certificates.

---

## 🔑 Types of Certificates in ACM

| Type        | Scope            | Renewal      | Use Case                    |
| ----------- | ---------------- | ------------ | --------------------------- |
| **Public**  | Internet-wide    | ✅ Auto-renew | Web apps via ALB/API GW     |
| **Private** | AWS internal PKI | ✅ Auto-renew | Internal apps, VPC services |

---

## 🌍 Use Cases

* Application Load Balancer (ALB)
* API Gateway (regional & edge)
* CloudFront
* Elastic Beanstalk

🚫 **Not supported** for use directly on **EC2** with public certs (you can't export ACM public certs).

---

## 🌐 HTTPS via ACM with ALB

When you host an app behind **ALB**, you:

1. Request a **public certificate** from ACM for your domain.
2. Attach it to ALB’s HTTPS listener (port 443).
3. Optionally redirect all HTTP traffic (port 80) to HTTPS.

➡️ **Encryption terminates at ALB**, then traffic forwarded unencrypted to EC2 unless re-encrypted internally.

---

## 🔁 How to Request a Public TLS Certificate

1. **Input domains** (FQDN or wildcard):
   e.g., `example.com`, `*.example.com`
2. **Validation type**:

   * ✅ **DNS validation** (preferred, automation-friendly)
   * 📧 Email validation (manual; emails sent to domain contacts)
3. **Domain validation**:

   * ACM gives a **CNAME record**
   * You place it in **Route 53** (automated) or your DNS provider
4. **Wait** until validation is confirmed
5. Certificate is issued and enrolled for **auto-renewal (60 days before expiry)**

---

## 🧭 Where Must You Create the ACM Certificate?

| Use Case                   | Region Requirement                  |
| -------------------------- | ----------------------------------- |
| CloudFront (Edge endpoint) | **`us-east-1` only**                |
| API Gateway (Regional)     | Must match **API Gateway’s region** |
| ALB, NLB                   | Same region as the load balancer    |

---

## 🔃 Certificate Renewal & Monitoring

### ✅ Auto-Renew (for ACM-issued public certs):

* ACM tries renewal **60 days before expiry**
* DNS must still be valid (CNAME record should remain)

### 🔔 Expiry Alerts

* **EventBridge** sends **daily events** starting **45 days** before expiry
* You can trigger:

  * Lambda
  * SNS
  * SQS

### 🛡 AWS Config Rule

* Rule: `acm-certificate-expiration-check`
* Triggers EventBridge events if a cert is **non-compliant** (i.e., near expiry)

---

## 📥 Importing External Certificates

* You can import certs created by **Let's Encrypt**, **GoDaddy**, etc.
* Must provide:

  * Public cert
  * Private key
  * CA chain
* ❌ No auto-renewal; must re-import manually before expiry

---

## 📡 ACM + API Gateway: Domain Customization

### 🧩 Step-by-Step:

1. Create a **Custom Domain Name** in API Gateway.
2. Attach ACM certificate.
3. Point Route 53 DNS (CNAME/alias) to the API endpoint.

### Endpoint Types:

| Type           | Global Clients  | CloudFront Used | ACM Region       |
| -------------- | --------------- | --------------- | ---------------- |
| Edge-Optimized | ✅ Yes           | ✅ Yes           | `us-east-1` only |
| Regional       | 🟡 Local only   | ❌ No (manual)   | Same as API GW   |
| Private        | ❌ No (VPC only) | ❌               | Same as API GW   |

---

## ⚙ ACM with CloudFront

* Certificates must exist in `us-east-1`
* Used for global TLS termination at edge locations
* Can redirect HTTP to HTTPS in CloudFront behavior settings

---

## 📦 ACM with Elastic Beanstalk

* Easy HTTPS setup via environment config
* ACM cert auto-attached to ALB behind the scenes

---

## 📌 Summary – ACM at a Glance

| Feature                | Supported by ACM               |
| ---------------------- | ------------------------------ |
| Auto-renewal           | ✅ (for public certs)           |
| DNS & Email validation | ✅                              |
| EC2 public cert usage  | ❌                              |
| Certificate monitoring | ✅ EventBridge + Config         |
| Free public certs      | ✅                              |
| TLS integration        | ✅ ALB, CloudFront, API Gateway |

---

Let me know if you’d like a **visual flowchart** for ACM setup or **mock questions** for practice.
