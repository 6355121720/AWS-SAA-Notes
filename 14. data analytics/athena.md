Here are the notes on **AWS Athena** based on Stephane Maarek’s AWS SAA-C03 slides:

---

## 🔍 **AWS Athena – Querying Data in S3 with SQL**

---

### ✅ What is Athena?

* **Serverless** interactive query service.
* Allows you to **analyze data in Amazon S3** using **standard SQL**.
* **No infrastructure** to manage.
* Based on **Presto** (open-source SQL engine) under the hood.

---

### 🗂️ How It Works

1. **Store raw data in S3** (CSV, JSON, Parquet, etc.).
2. Define a **schema** using the AWS Glue Data Catalog (or manually).
3. Run **SQL queries** on the data directly in S3 using Athena.
4. **Pay-per-query**: You pay for the **amount of data scanned**.

---

### 💡 Key Features

| Feature                  | Description                                       |
| ------------------------ | ------------------------------------------------- |
| **Serverless**           | No servers or clusters to provision or manage     |
| **SQL Support**          | Standard ANSI SQL syntax for querying             |
| **Fast & Interactive**   | Ideal for ad-hoc querying and data exploration    |
| **Integrates with Glue** | Uses AWS Glue as a schema/catalog manager         |
| **Secure**               | Supports encryption (S3 SSE), IAM, VPC, KMS       |
| **Output to S3**         | Query results are stored in a specified S3 bucket |

---

### 📦 Supported Formats

* CSV, JSON, ORC, Parquet, Avro, and more.

---

### 💰 Pricing

* Charged **per TB of data scanned**.
* **Optimize cost**: Convert to **columnar formats** (Parquet, ORC) and **partition your data**.

---

### 📌 Common Use Cases

* Ad-hoc data analysis
* Analyzing logs (e.g., CloudTrail, ALB logs)
* Quick exploration of S3-stored data
* Business intelligence reporting
* ETL pre-processing or validation

---

### 🧠 Optimization Tips

| Tip                      | Benefit                           |
| ------------------------ | --------------------------------- |
| **Partition data**       | Reduces scanned data, lowers cost |
| **Use columnar formats** | Parquet/ORC → faster + cheaper    |
| **Use Glue Catalog**     | Simplifies schema management      |

---

### 🆚 When to Use Athena vs. Redshift

| Scenario                     | Use                                  |
| ---------------------------- | ------------------------------------ |
| Ad-hoc SQL queries on S3     | **Athena**                           |
| Complex analytics, joins, BI | **Redshift (OLAP)**                  |
| Real-time dashboards on logs | **Athena + QuickSight / OpenSearch** |

---

Let me know if you'd like Athena use case examples, Glue integration, or comparison with Redshift Spectrum.
