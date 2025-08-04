Here’s the **refined and structured version** of your **SSM Additional Services notes** in my style — fully exam-ready, short yet deep:

---

## 🛠️ AWS Systems Manager – Other Key Features

---

### 1. 📥 Run Command

**What it is:**
Run shell scripts or PowerShell commands on **multiple EC2 or on-prem servers** — **without SSH**.

**How it works:**

* Uses **SSM Agent** (must be running).
* Sends commands via AWS Console, CLI, SDK, or triggered by EventBridge.
* Command output can go to **S3 or CloudWatch Logs**.
* Supports **notifications via SNS** on status (in-progress, success, failed).
* Fully **IAM-integrated** (you control who can run what).
* Logged in **CloudTrail** (audit who ran which command).

**Use Case:**
Install software, restart services, diagnose logs — all remotely.

---

### 2. 🔄 Patch Manager

**What it is:**
Fully automates **OS & software patching** on EC2 and on-prem servers (Linux, Windows, Mac).

**How it works:**

* Uses **AWS-RunPatchBaseline** document to apply patches.
* Schedule via **Maintenance Windows** or run ad hoc.
* Generates **patch compliance reports** (which servers are missing patches).
* Choose to **scan only** or scan & patch.

**Use Case:**
Keep fleet secure and compliant with minimal effort.

---

### 3. 🧰 Maintenance Windows

**What it is:**
Lets you define **time windows** to safely apply changes (patches, updates, scripts).

**Key elements:**

* **Schedule:** Daily, weekly, cron-like.
* **Duration:** How long the window stays open.
* **Targets:** Which instances or resource groups to act on.
* **Tasks:** What actions to perform (Run Command, Automation, etc).

**Use Case:**
Apply updates every Sunday 2–4 AM on production servers.

---

### 4. 🔁 Automation

**What it is:**
Run **predefined workflows (runbooks)** to automate repetitive tasks — with or without user input.

**Examples:**

* Restart EC2 instances.
* Create AMI snapshots.
* Take RDS/EBS backups.
* Auto-remediate non-compliant resources.

**Triggers:**

* Manual (Console, CLI, SDK).
* EventBridge.
* Maintenance Windows.
* AWS Config → for auto-remediation when rules are broken.

**Uses runbooks like:**

* `AWS-StartEC2Instance`, `AWS-StopEC2Instance`, `AWS-CreateImage`, etc.

---

## ✅ Summary: SSM Power Tools

| Feature                 | Purpose                               | Notes                          |
| ----------------------- | ------------------------------------- | ------------------------------ |
| **Run Command**         | Run scripts/commands remotely         | No SSH needed, logs to S3/CW   |
| **Patch Manager**       | Auto-patch OS/apps                    | Scheduled or on-demand         |
| **Maintenance Windows** | Define time-based control for changes | Helps enforce downtime periods |
| **Automation**          | Workflow automation on AWS resources  | Great for compliance & DR      |

---

Let me know if you want a visual cheat sheet or YAML CloudFormation examples for any of these.
