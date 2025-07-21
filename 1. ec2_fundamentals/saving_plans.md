
---

## 🧠 First: What Are Savings Plans?

> Savings Plans are a **flexible alternative** to RIs.
> You commit to **spending a fixed \$/hour amount** (e.g., \$5/hr) for **1 or 3 years**, and AWS gives **up to 72% discount** on usage that matches.

You pay the **committed discounted rate**, even if you don’t use it fully.

if i switch to less expensive instance type, then maybe i am paying almost on-demand for it in most cases

---

## 🔹 1. **Compute Savings Plan** — Most Flexible

* ✅ Apply to **any compute usage**: EC2, Fargate, Lambda
* ✅ Works across **all instance types, sizes, families, regions, OS**
* ✅ Switch freely between instance types/regions
* ❌ Slightly lower discount (\~66% max)

### 💡 Use if:

You want **maximum flexibility** — your infra might change in future.

---

## 🔸 2. **EC2 Instance Savings Plan** — Higher Discount, Less Flexible

* ✅ Higher discount (\~72%)
* ✅ You choose: **Instance Family + Region** (e.g., `m5` in `us-east-1`)
* ✅ You can switch size (e.g., `m5.large` ↔ `m5.4xlarge`), OS, tenancy
* ❌ Locked to **that family and region**
* ❌ Doesn't apply to Fargate/Lambda

### 💡 Use if:

You run **EC2 only**, and know **which family and region** you’ll use for 1–3 years.

---

## 📊 Comparison Table

| Feature                   | **Compute Savings Plan** | **EC2 Instance Savings Plan** |
| ------------------------- | ------------------------ | ----------------------------- |
| Max Discount              | \~66%                    | **\~72%**                     |
| Flexibility (type/region) | ✅ Any type/region/family | ❌ Fixed family & region       |
| Applies To                | EC2, Lambda, Fargate     | EC2 only                      |
| Best For                  | Dynamic workloads        | Stable EC2 infra              |

---

## 🔑 Key Difference

\| You commit to...            | ✅ A **\$/hour spend** (not instance type) |
\| Pay discounted rate on...   | ✅ Usage matching your plan               |
\| Under-usage?                | ❌ You still pay the **full \$/hr** committed

---
