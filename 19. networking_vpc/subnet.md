Of course! Here's the **Subnet Overview** rewritten in my structured, simplified, and to-the-point format:

---

## 🧩 Subnet Overview — Inside a VPC

### 🔹 What is a Subnet?

A **subnet** is a smaller slice of your VPC’s IP range (CIDR block). It divides your VPC into isolated network segments — like rooms inside a house.

You can create:

* 🔓 **Public subnet** → Has internet access via a route to an **Internet Gateway**
* 🔒 **Private subnet** → No direct access to internet (can use NAT Gateway for outbound)

---

### 🔸 Reserved IPs in Each Subnet

AWS reserves **5 IPs per subnet** — not usable for EC2 instances:

| IP Type               | Example (`10.0.0.0/24`) | Description                                            |
| --------------------- | ----------------------- | ------------------------------------------------------ |
| **Network address**   | `10.0.0.0`              | Identifies the subnet                                  |
| **VPC Router**        | `10.0.0.1`              | Used internally by AWS networking                      |
| **Amazon DNS**        | `10.0.0.2`              | Maps internal DNS                                      |
| **Future use**        | `10.0.0.3`              | Reserved by AWS                                        |
| **Broadcast address** | `10.0.0.255`            | Not used, but reserved (AWS doesn’t support broadcast) |

🔹 So, in a `/24` subnet (256 IPs), you can use only **251** IPs.

---

### ⚠️ Exam Tip: Subnet Sizing

Always subtract **5 reserved IPs** when planning subnet size!

✅ Example:

* Need **29 usable IPs** →
  ❌ `/27` gives only 32 total IPs → 32 - 5 = **27 usable** → too small
  ✅ Use `/26` → 64 total → 64 - 5 = **59 usable** → enough

---

### ✅ Summary

* A **subnet** is a portion of your VPC’s IP space
* AWS **reserves 5 IPs** per subnet (always account for this!)
* **Subnet size matters** — plan for future growth, NAT, and scaling
* Subnets can be **public or private** depending on the **route table**

Let me know if you want a public/private subnet setup diagram next!
