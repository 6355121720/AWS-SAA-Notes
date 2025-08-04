## 🔐 How Encryption at Rest Works in S3

### 🧱 Basic Concept

* Data is **stored encrypted** on disk (in S3).
* When you upload data, AWS **encrypts it before writing to disk**.
* When you download data, AWS **decrypts it before sending it to you**.
* Encryption is done using **data encryption keys (DEKs)**.

But what controls the DEK?
That depends on whether you're using `SSE-S3` or `SSE-KMS`.

---

## ⚙️ SSE-S3 (Server-Side Encryption with Amazon S3-Managed Keys)

### 🔑 Key Management:

* AWS **manages the keys for you**.
* A **single symmetric key** (AES-256) is used internally by S3.
* You **don’t see** or manage this key.

### 🔄 How It Works:

1. You upload an object to S3.
2. S3 generates a **one-time random DEK (Data Encryption Key)** for that object.
3. The object is encrypted using that DEK with AES-256.
4. The DEK is then encrypted using **S3’s master key** (managed by AWS).
5. The encrypted object + encrypted DEK are stored together.

🔓 On retrieval:

* S3 decrypts the DEK using the master key.
* Then decrypts the object using the DEK.
* Sends you the plaintext.

> ✅ All of this is automatic and invisible to you.

---

## 🔐 SSE-KMS (Server-Side Encryption with AWS Key Management Service)

### 🔑 Key Management:

* Uses **KMS CMKs (Customer Master Keys)**.
* You can use:

  * AWS-managed KMS keys (e.g., `aws/s3`)
  * or your own customer-managed keys (CMKs).

### 🔄 How It Works:

1. You upload an object.
2. S3 asks KMS to generate a **unique DEK** for your object.
3. KMS returns:

   * A plaintext DEK (for immediate encryption)
   * An encrypted copy of the DEK (using your CMK)
4. S3 encrypts the object with the **plaintext DEK**.
5. S3 stores:

   * The encrypted object
   * The **encrypted DEK** (encrypted with your KMS CMK)

🔓 On retrieval:

* S3 sends the encrypted DEK to KMS.
* KMS decrypts the DEK (after verifying IAM permissions).
* S3 decrypts the object and returns plaintext to you.

---

## 🔍 Key Differences: SSE-S3 vs SSE-KMS

| Feature                | SSE-S3                         | SSE-KMS                                  |
| ---------------------- | ------------------------------ | ---------------------------------------- |
| Key Ownership          | AWS S3                         | AWS KMS                                  |
| Key Type               | Single symmetric key (AES-256) | Symmetric key (CMK) + per-object DEKs    |
| Visibility to Customer | No access to keys              | You can manage/rotate CMKs               |
| Audit via CloudTrail   | ❌ No                           | ✅ Yes (logs all use of CMK)              |
| Key Rotation           | Handled by AWS silently        | ✅ Automatic or manual rotation available |
| Access Control         | IAM only                       | IAM + KMS Key Policies                   |
| Cost                   | Free                           | Small cost per KMS request + CMK charges |

---


## 🧪 Real-World Analogy

**SSE-S3**:
Like putting your valuables in a locker, and AWS holds both the lock and key. You trust them entirely.

**SSE-KMS**:
You still use AWS’s locker, but you bring your own lock (KMS CMK). AWS must ask your permission (via KMS) every time it opens the locker.

---

## ✅ Summary

| Feature            | SSE-S3              | SSE-KMS                             |
| ------------------ | ------------------- | ----------------------------------- |
| Key Type           | Symmetric (AES-256) | Symmetric (DEK + CMK via KMS)       |
| Key Visibility     | Hidden              | Managed by you via KMS              |
| Key Control        | None                | Fine-grained control with IAM + KMS |
| Cost               | Free                | \$ per KMS call + CMK storage cost  |
| Encryption Style   | Symmetric only      | Symmetric only                      |
| Public/Private Key | ❌ Not used          | ❌ Not used                          |