Here’s the **Encrypted AMI Sharing Process** explained in a **clear, visual, real-world style** for deep understanding (and exam prep):

---

## 🔐 Encrypted AMI Sharing – Step-by-Step Flow

### 🎯 Goal:

You want to **share an AMI from Account A to Account B**, but it's **encrypted using a KMS key in Account A**.

---

### 🧭 High-Level Steps

| Step | Action                                           | Who Does It          | Why                                                |
| ---- | ------------------------------------------------ | -------------------- | -------------------------------------------------- |
| 1️⃣  | **Modify AMI launch permissions**                | ✅ Account A          | Allows Account B to see & launch the AMI           |
| 2️⃣  | **Share the KMS key (used to encrypt AMI)**      | ✅ Account A          | Account B needs access to decrypt the EBS snapshot |
| 3️⃣  | **Grant IAM permissions in Account B**           | ✅ Account B          | To actually launch EC2 using that AMI              |
| 4️⃣  | ✅ **Optionally re-encrypt** with its own KMS key | (Optional) Account B | For key control and regional compliance            |

---

## 🔧 Detailed Breakdown

### ✅ Step 1: Share the AMI (Launch Permission)

> 📍 Command/API: `ModifyImageAttribute`

```bash
aws ec2 modify-image-attribute \
  --image-id ami-1234567890abcdef0 \
  --launch-permission "Add=[{UserId=222222222222}]"
```

* This makes the **AMI visible** to Account B.
* But it's still **encrypted with Account A's KMS key** — so Account B can't launch it yet.

---

### ✅ Step 2: Share the KMS Key (Key Policy in Account A)

Update the **KMS key policy** in Account A to grant usage to Account B:

```json
{
  "Sid": "AllowAccountBUseOfKey",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::222222222222:root"
  },
  "Action": [
    "kms:Decrypt",
    "kms:ReEncrypt*",
    "kms:CreateGrant",
    "kms:DescribeKey"
  ],
  "Resource": "*"
}
```

> 🔐 This allows Account B to decrypt the AMI’s underlying snapshot.

---

### ✅ Step 3: IAM Role/User in Account B

Make sure Account B has an **IAM role or user** with permissions like:

```json
{
  "Effect": "Allow",
  "Action": [
    "ec2:RunInstances",
    "ec2:DescribeImages",
    "kms:Decrypt",
    "kms:ReEncrypt*",
    "kms:DescribeKey"
  ],
  "Resource": "*"
}
```

> ✅ Needed to:

* **Launch EC2** from the shared AMI
* **Decrypt** volumes during boot time using KMS

---

### ✅ Step 4: (Optional) Re-encrypt with Account B's Own KMS Key

Once launched, Account B can create **a new AMI or snapshot** re-encrypted with **its own KMS key**:

```bash
aws ec2 create-image --instance-id i-abc123 --name "NewAMIWithOwnKey"
```

This is optional but:

* ✅ Gives full key control
* ✅ Better isolation
* ✅ Required for AMI replication in some scenarios

---

## 📌 Summary Table

| What to Share                | Who Does It | Why                             |
| ---------------------------- | ----------- | ------------------------------- |
| AMI Launch Permission        | Account A   | So Account B can see and launch |
| KMS Key Access (Policy)      | Account A   | So B can decrypt the AMI        |
| IAM Role/User with KMS perms | Account B   | So EC2 can be launched          |
| (Optional) Re-encryption     | Account B   | Full ownership of encryption    |

---

## 🧠 Final Tip for Exam

> Sharing an **unencrypted AMI** only needs launch permissions.
> But sharing an **encrypted AMI** **always** needs:

* 🔓 Launch permission
* 🔐 KMS key access

---

Let me know if you want CLI-only or CloudFormation versions of this process.
