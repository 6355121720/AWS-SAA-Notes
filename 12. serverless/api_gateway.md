Absolutely! Here's the **updated version** of your **Amazon API Gateway notes**, now with **deployment modes** (`Edge-Optimized`, `Regional`, and `Private`) added clearly under a dedicated section. The rest is unchanged for continuity and clarity.

---

# 🛡️ Amazon API Gateway — In-Depth Notes

---

## 🚪 What is API Gateway?

Amazon API Gateway is a **fully managed service** to:

* **Create**, **publish**, **secure**, **monitor**, and **scale** **APIs** (REST, HTTP, WebSocket)
* Acts as a **“front door”** to your backend services (Lambda, EC2, ECS, any HTTP endpoint)

---

## ✅ Supported API Types

| Type          | Purpose                                 | Protocol  |
| ------------- | --------------------------------------- | --------- |
| **REST API**  | Legacy, feature-rich (IAM, usage plans) | HTTPS     |
| **HTTP API**  | Newer, lightweight, faster, cheaper     | HTTPS     |
| **WebSocket** | Real-time bidirectional communication   | WebSocket |

---

## ⚙️ API Gateway Core Features

| Feature              | Explanation                                                              |
| -------------------- | ------------------------------------------------------------------------ |
| **Routing**          | Maps URL paths and HTTP methods to backend (e.g., `/user POST → Lambda`) |
| **Authentication**   | IAM, Cognito User Pools, API Keys, Lambda authorizers                    |
| **Throttling**       | Prevents abuse by setting request limits (global or per API key)         |
| **Caching**          | Response caching at edge (CloudFront) to reduce latency and backend load |
| **Monitoring**       | Integrated with **CloudWatch** (logs, metrics, alarms)                   |
| **SDK Generation**   | Auto-generates client SDKs (Android, iOS, JavaScript)                    |
| **CORS Support**     | Enables secure cross-origin requests from web apps                       |
| **Stage Deployment** | Deploy different versions (dev, test, prod)                              |
| **Custom Domains**   | Use your own domain names (e.g., api.myapp.com)                          |

---

## 🔒 Authentication & Authorization

| Method                 | Use Case                                   |
| ---------------------- | ------------------------------------------ |
| **IAM Roles/Policies** | For **internal AWS services/users**        |
| **API Keys**           | For **basic access control** + usage plans |
| **Cognito User Pools** | For **user sign-in**, token-based auth     |
| **Lambda Authorizers** | Custom token validation logic (JWT, etc.)  |

---

## 🔁 Integration Types

| Type            | Description                                       |
| --------------- | ------------------------------------------------- |
| **Lambda**      | Most common — triggers AWS Lambda function        |
| **HTTP**        | Forward to public/private HTTP backend            |
| **AWS Service** | Invoke AWS services directly (e.g., S3, DynamoDB) |
| **Mock**        | Return mock responses without calling backend     |
| **VPC Link**    | Access private resources in VPC (via NLB)         |

---

## 🚀 API Deployment Modes (for **REST APIs** only)

| Mode               | Description                                                                 |
| ------------------ | --------------------------------------------------------------------------- |
| **Edge-Optimized** | Default. Best for **global clients**. Uses CloudFront to distribute traffic |
| **Regional**       | Best for **clients in a specific AWS region**. No CloudFront involved       |
| **Private**        | Accessible **only within your VPC** via **VPC endpoints (interface)**       |

> 📌 **HTTP APIs** only support **Regional** mode (no Edge or Private).

---

## 📦 Deployment Flow

1. **Create API**
2. Define **resources** (e.g., `/users`)
3. Define **methods** (`GET`, `POST`, etc.)
4. Set **integration target** (Lambda, HTTP, etc.)
5. Add **auth, throttling, logging**
6. Deploy to a **stage** (e.g., `/prod`)
7. Access via **invoke URL** (depends on deployment mode)

---

## 📈 Monitoring & Logging

* **CloudWatch Logs**: Full request/response logs
* **CloudWatch Metrics**: Request count, errors, latency
* **X-Ray Tracing**: End-to-end tracing of requests

---

## 💰 Pricing (HTTP/REST)

* Charged **per API call**
* Extra charges for:

  * Data transfer
  * Caching
  * Authorizers (e.g., Lambda execution)
  * VPC Link usage

> 🧠 **HTTP APIs are \~70% cheaper** and faster than REST APIs for basic needs.

---

## 🚀 Real-World Example

A mobile app hits `https://api.myapp.com/register`:

* API Gateway receives `POST /register`
* Authenticates via **Cognito**
* Triggers **Lambda**
* Stores data in **DynamoDB**
* Returns response to client
* Logs to **CloudWatch**

---

## 🧠 Tips for SAA Exam

* **API Gateway + Lambda** = Serverless backend
* **Use Cognito/Lambda authorizers** for token validation
* **Enable caching** for read-heavy APIs to reduce latency
* Know difference between **HTTP API vs REST API**
* Use **VPC Link** to securely connect to private EC2/ECS services
* Choose deployment mode (**Edge, Regional, Private**) based on where clients are and how secure you want access to be

---

Let me know if you'd like a quick comparison between **HTTP API vs REST API vs WebSocket API** or a diagram!
