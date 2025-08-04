Here’s a clean, simplified explanation of **NAT Instances** in AWS:

---

### 🔄 NAT Instance – Overview

A **NAT (Network Address Translation) Instance** lets **private subnet EC2 instances** access the internet **without exposing them to inbound traffic**.

---

### 🛠️ How to Set Up a NAT Instance (Step-by-Step)

#### 1. **Launch a NAT Instance**

* Go to EC2 → Launch Instance → Search for **"NAT"** under **Community AMIs**.
* Choose an official **x86\_64** NAT AMI (e.g., *Amazon AMI VPC NAT 2018*).
* Use **t2.micro** and place it in a **public subnet**.
* Assign an **Elastic IP** to the NAT instance.

#### 2. **Security Group Configuration**

Allow from VPC CIDR (e.g., `10.0.0.0/16`):

* ✅ SSH (from anywhere or your IP)
* ✅ HTTP & HTTPS (from VPC)
* ✅ ICMP (for `ping`)

#### 3. **Disable Source/Destination Check**

* Right-click NAT instance → **Networking** → **Change Source/Dest Check** → **Disable**
* This allows it to forward traffic.

#### 4. **Update Route Table of Private Subnet**

* Add route:

  * **Destination**: `0.0.0.0/0`
  * **Target**: NAT instance ID

#### 5. **Test Internet Access**

* SSH into **Private Instance** via **Bastion Host**.
* Try:

  ```bash
  ping google.com
  curl example.com
  ```

---

### ✅ Summary

| Step                          | Purpose                                                |
| ----------------------------- | ------------------------------------------------------ |
| NAT Instance in Public Subnet | Acts as internet gateway for private subnets           |
| Disable Src/Dst Check         | Allows NAT instance to route traffic                   |
| Update Route Table            | Sends outbound traffic from private subnet through NAT |
| Secure with SG Rules          | Controls allowed traffic types                         |

---

### 🔁 What’s Next?

👉 NAT Instances are outdated. Next up: **NAT Gateway** – a fully managed, highly available alternative.

Let me know if you want a quick comparison between NAT Instance and NAT Gateway.
