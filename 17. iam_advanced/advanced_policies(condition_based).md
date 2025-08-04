Absolutely! Here's a **complete and in-depth guide** to **Advanced IAM Conditions and Policies**, rewritten in **my structured style** and **expanded with key practical insights**, combining real use cases and exam-level understanding:

---

# ✅ **IAM Advanced Policies & Conditions — In-Depth Notes**

---

## 🧠 What Are IAM Conditions?

**IAM Conditions** are powerful filters in IAM and resource policies that control *when* and *under what context* access is allowed or denied.

They help answer:

> ❓“Can this action happen *only if* X is true?”

Conditions are used in:

* IAM user/role policies
* S3 bucket policies
* KMS key policies
* VPC endpoint policies
* Service Control Policies (SCPs)
* And more

---

## 🔒 Common IAM Condition Keys and Use Cases

---

### 1. ✅ **`aws:SourceIp`** — Restrict by IP Address

**Purpose**: Limit API calls to a specific **CIDR/IP range**
🔐 Helps restrict access to your corporate network

#### ✅ Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "NotIpAddress": {
          "aws:SourceIp": "203.0.113.0/24"
        }
      }
    }
  ]
}
```

🧠 Use this to:

* Allow access **only from VPN/corporate offices**
* Prevent usage from unknown locations

---

### 2. 🌍 **`aws:RequestedRegion`** — Restrict Actions by Region

**Purpose**: Allow or deny actions based on the **region the API call is made in**

#### ✅ Example (deny access to Europe):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "eu-west-1"
        }
      }
    }
  ]
}
```

🧠 Practical uses:

* Lock workloads to specific **regions for compliance**
* Prevent **accidental resource creation** in unintended regions
* Can be enforced via **SCPs** as well

---

### 3. 🔖 **`ec2:ResourceTag`** — Tag-Based Resource Control

**Purpose**: Enforce actions only on resources with specific **tags**

#### ✅ Example (only manage instances tagged `Project=DataAnalytics`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Project": "DataAnalytics"
        }
      }
    }
  ]
}
```

🧠 Enforce:

* Project-level access
* Billing accountability
* Resource ownership boundaries

---

### 4. 🧑‍💼 **`aws:PrincipalTag`** — User/Role Tag-Based Access

**Purpose**: Control actions based on **who the principal is (tags on IAM identity)**

#### ✅ Example (allow access only if the user has `Department=Data`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalTag/Department": "Data"
        }
      }
    }
  ]
}
```

🧠 Use cases:

* Attribute-based access control (ABAC)
* Auto-provisioned roles tied to departments

---

### 5. 🔐 **`aws:MultiFactorAuthPresent`** — Enforce MFA for Sensitive Actions

**Purpose**: Require MFA for specific actions like stopping EC2, accessing secrets, etc.

#### ✅ Example (deny sensitive actions **if MFA not used**):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": "*",
      "Condition": {
        "Bool": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```

🧠 Use it to:

* Enforce **step-up security**
* Prevent account compromise in case of stolen credentials

---

### 6. 🏢 **`aws:PrincipalOrgID`** — Restrict Access to Your Organization Only

**Purpose**: Allow access **only if the request comes from your AWS Organization**

#### ✅ Example (S3 bucket policy):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowFromMyOrgOnly",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:PrincipalOrgID": "o-xxxxxxxxxx"
        }
      }
    }
  ]
}
```
🧠 Use this to:

* Protect S3 buckets, KMS keys, Lambda functions
* Ensure **only internal accounts** can use the resource
* Works **even across accounts** inside your org

---

## 🪣 S3-Specific IAM Considerations

### 🗂️ Bucket-level vs Object-level ARNs:

| Permission Type | ARN Format                   | Example Action                           |
| --------------- | ---------------------------- | ---------------------------------------- |
| Bucket-level    | `arn:aws:s3:::bucket-name`   | `ListBucket`, `GetBucketPolicy`          |
| Object-level    | `arn:aws:s3:::bucket-name/*` | `GetObject`, `PutObject`, `DeleteObject` |

🧠 You must match the **correct ARN level** in your policies, or access will fail.

---

## 🔐 Real-World IAM Security Use Cases (Combining Conditions)

| Use Case                                           | IAM Condition Strategy           |
| -------------------------------------------------- | -------------------------------- |
| Restrict access to internal users only             | `aws:SourceIp` with company CIDR |
| Allow access only in `us-east-1`                   | `aws:RequestedRegion`            |
| Allow users to access only their department’s data | `aws:PrincipalTag + ResourceTag` |
| Enforce MFA for resource deletion                  | `aws:MultiFactorAuthPresent`     |
| Secure cross-account S3 access                     | `aws:PrincipalOrgID`             |

---

## 🔐 Bonus: Conditions in SCPs

IAM Conditions like `aws:RequestedRegion` and `aws:PrincipalOrgID` can also be used in **SCPs** to:

* Deny access to non-compliant regions
* Enforce organization-wide network/location rules
* Ensure SCP applies **only when condition is met**

---

## ✅ Summary — Core IAM Condition Capabilities

| Condition Key                | Purpose                                       |
| ---------------------------- | --------------------------------------------- |
| `aws:SourceIp`               | Restrict API calls by IP/CIDR                 |
| `aws:RequestedRegion`        | Restrict by AWS region                        |
| `ec2:ResourceTag`            | Allow/deny based on EC2 resource tags         |
| `aws:PrincipalTag`           | Allow/deny based on user or role tag          |
| `aws:MultiFactorAuthPresent` | Enforce MFA-based actions                     |
| `aws:PrincipalOrgID`         | Restrict access to only your AWS Organization |

---

Would you like:

* ✅ Practice questions on IAM conditions?
* ✅ Visual diagrams for access evaluation with conditions?
* ✅ Combined SCP + IAM condition strategy for real-world org security?

Let me know — I can walk you through advanced use cases step-by-step.
