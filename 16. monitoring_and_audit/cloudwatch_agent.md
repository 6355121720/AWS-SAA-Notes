Here's the **deeply structured, industry-standard breakdown** of **CloudWatch Logs Agent vs. CloudWatch Unified Agent**, including their use cases, architecture, metrics, IAM, and real-world setups — **exactly in my style**:

---

# ✅ CloudWatch Agent vs Logs Agent – Deep Dive for EC2 and On-Prem

## 🧠 Why Do We Need Agents?

By **default**, EC2 instances (and on-prem servers) **do not send logs or system-level metrics** to CloudWatch.
To enable this, you install **CloudWatch agents** — lightweight daemons that run on your servers and:

🔸 **Push log files** to CloudWatch Logs
🔸 **Send detailed OS metrics** (RAM, disk, network, processes) to CloudWatch Metrics
🔸 Can work with **on-prem servers**, not just EC2

---

## 🆚 CloudWatch Logs Agent vs CloudWatch Unified Agent

| Feature                           | **CloudWatch Logs Agent** | **CloudWatch Unified Agent**    |
| --------------------------------- | ------------------------- | ------------------------------- |
| **Purpose**                       | Logs only                 | Logs **+** system-level metrics |
| **Logs Supported**                | ✅ Yes                     | ✅ Yes                           |
| **System Metrics (RAM, Disk IO)** | ❌ No                      | ✅ Yes                           |
| **Installable on-prem**           | ✅ Yes                     | ✅ Yes                           |
| **EC2 Ready**                     | ✅ Yes                     | ✅ Yes                           |
| **SSM Parameter Store Config**    | ❌ No                      | ✅ Yes (centralized config)      |
| **Recommended by AWS**            | ❌ Deprecated              | ✅ Yes (modern & flexible)       |

> ✅ **Use CloudWatch Unified Agent for all new setups**
> ❌ **Logs Agent is deprecated** and only exists for legacy support

---

## ⚙️ CloudWatch Unified Agent – Capabilities

### ✅ Sends to:

* **CloudWatch Logs**: `/var/log/syslog`, `/var/log/nginx/access.log`, etc.
* **CloudWatch Metrics**: memory, disk, CPU, processes, network

### 🧠 Collects Highly Granular Metrics:

| Metric Type    | Details                                                         |
| -------------- | --------------------------------------------------------------- |
| **CPU**        | User, system, idle, steal, iowait, guest                        |
| **RAM**        | Free, used, total, cached, inactive                             |
| **Disk**       | Read/write ops, throughput (bytes), disk space usage            |
| **Network**    | TCP/UDP connections, packets in/out, dropped, bytes transferred |
| **Processes**  | Total, running, sleeping, idle, zombie, blocked                 |
| **Swap Space** | Total, used, free, % used (for memory overflow detection)       |

> 🧪 These are **not available by default** with EC2 monitoring – only possible with Unified Agent.

---

## 🏗️ Agent Installation – EC2 or On-Prem

### ✅ Install on:

* EC2 Linux/Windows
* On-prem servers (e.g., VMware, bare-metal)
* Edge devices or hybrid environments

### ⚠️ IAM Role Requirements:

The EC2 instance (or hybrid server identity) must have **IAM permissions** to:

```json
{
  "Action": [
    "logs:PutLogEvents",
    "logs:CreateLogStream",
    "logs:CreateLogGroup",
    "cloudwatch:PutMetricData",
    "ssm:GetParameter"
  ],
  "Effect": "Allow",
  "Resource": "*"
}
```

---

## 🧩 Config Management – SSM Parameter Store

The **Unified Agent** supports storing its entire config as a JSON document in **AWS Systems Manager Parameter Store**.

Benefits:

* 💼 Centralized control for 100s of EC2 instances
* ♻️ Automatically picked up during bootstrapping
* 🛡️ Easier auditing and automation (e.g., via Terraform or CloudFormation)

### 🧾 Example Config in Parameter Store:

```json
{
  "metrics": {
    "namespace": "MyApp/EC2",
    "metrics_collected": {
      "mem": { "measurement": ["used_percent"] },
      "cpu": { "measurement": ["idle", "user", "system"] }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/nginx/access.log",
            "log_group_name": "/app/nginx/access"
          }
        ]
      }
    }
  }
}
```

---

## 📈 Real-World Workflow Example

### Goal: Collect system metrics + app logs from EC2 and send them to CloudWatch

✅ Setup:

1. EC2 with IAM role allowing Logs + CloudWatch access
2. Install **Unified Agent** via SSM or bootstrap script
3. Define config in **SSM Parameter Store**
4. Agent reads config and starts collecting logs + metrics
5. Data appears in:

   * **CloudWatch Logs** → `/application/orders-api`
   * **CloudWatch Metrics** → Namespace: `CustomEC2Metrics`
6. Use Logs Insights + Metric Alarms for alerting & dashboarding

---

## 🔑 Key Takeaways

| Concept              | Summary                                                              |
| -------------------- | -------------------------------------------------------------------- |
| **Unified Agent**    | Modern agent that supports both logs and metrics                     |
| **Logs Agent**       | Legacy agent (logs only) – avoid unless maintaining old workloads    |
| **Granular Metrics** | RAM, CPU (iowait, guest, steal), swap, processes, disk IO, network   |
| **Cross-platform**   | Works on EC2 **and** on-prem (via hybrid activation or manual setup) |
| **SSM Config**       | Store JSON config for automation and mass-deployment                 |
| **IAM Required**     | Must allow Logs + PutMetricData + optional SSM\:GetParameter         |

---

Let me know if you want:

* 🧪 Step-by-step install for Unified Agent
* 📈 Dashboard or alarm setup using these custom metrics
* 🛠️ Hybrid (on-prem) setup with CloudWatch Agent
* 📦 Export logs from EC2 → CloudWatch → S3

I can also generate a full real-world diagram of how EC2 agents push logs/metrics across accounts or regions.
