Here's the **clear, structured, real-world-style version** of how to **block an IP in AWS**, broken into logical security layers:

---

## 🔐 How to Block an IP Address in AWS (with Real-World Architecture)

---

### ✅ 1. **First Line of Defense: Network ACL (NACL)**

* **Works at the subnet level** (stateless).
* Can **explicitly DENY traffic** from a specific IP or IP range (`CIDR`).
* ✅ Use when your **EC2 is in a public subnet**.
* 🧠 NACL rules are evaluated **in order**, so **deny first, allow later**.

```plaintext
Rule #: 100 | Deny | Source: 203.0.113.45/32 | Port: 80 | Protocol: TCP
```

📌 Example:

* Block attacker IP `203.0.113.45` from accessing anything in your subnet.

---

### ✅ 2. **Second Line: Security Groups (EC2/ALB level)**

* **Works at instance level** (stateful).
* Only support **allow rules** – no deny.
* Best for **whitelisting known trusted IPs**.
* Doesn't block — instead, **restricts who can connect**.

📌 Example:

* Only allow SSH (22) from your admin IP `198.51.100.10/32`.

```plaintext
Inbound Rule:
Type: SSH | Protocol: TCP | Port: 22 | Source: 198.51.100.10/32
```

---

### ✅ 3. **Third Line: Host-based Firewall (Inside EC2)**

* Tools like **iptables**, **ufw**, or **firewalld**.
* You can **inspect and drop packets at OS level**.
* Useful for **fine-grained control**, logging, or dynamic blocking.
* Costly for high-throughput apps — use only if really needed.

---

### ✅ 4. **ALB (Application Load Balancer) Architecture**

* Deploy ALB in **public subnet**, EC2 in **private subnet**.
* ALB terminates connection, forwards to EC2 securely.
* EC2's security group should **only allow ALB’s SG**, not the world.

📌 Best Practice:

* Clients hit ALB → ALB filters traffic → forwards to EC2 only if allowed.

---

### ✅ 5. **WAF (Web Application Firewall) on ALB or CloudFront**

* **Advanced filtering**: block by IP, country, rate limiting, bad patterns.
* WAF sits **in front of ALB** (or CloudFront) and filters malicious requests.

📌 Example WAF Rules:

* Block IP `203.0.113.45`.
* Block traffic from country `CN` (China).
* Block SQL injection patterns.

---

### ✅ 6. **CloudFront + WAF (if using CDN)**

* CloudFront edge locations forward traffic to ALB.
* The **real client IP is not visible to NACL**, only CloudFront IP.
* You must **allow CloudFront IPs** in ALB’s SG.
* WAF is **mandatory here for client-level filtering**.

---

## 🧠 Summary: Where to Block What

| Layer            | Use When                           | Can Deny? | Example Use                   |
| ---------------- | ---------------------------------- | --------- | ----------------------------- |
| NACL             | EC2 in Public Subnet               | ✅ Yes     | Block a known bad IP          |
| Security Group   | Control who can connect to EC2/ALB | ❌ No      | Allow only internal IPs       |
| OS Firewall      | Deep control on EC2 instance       | ✅ Yes     | Block app-level abuse         |
| ALB + WAF        | Centralized app-level security     | ✅ Yes     | Block countries/IP patterns   |
| CloudFront + WAF | Serving static/dynamic content     | ✅ Yes     | Block bots, scrapers, attacks |

---

Let me know if you want a diagram or example rule set for any of the layers.
