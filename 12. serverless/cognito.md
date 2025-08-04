

### 🔐 **AWS Cognito — Real Stuff, No Fluff**

---

## 🔹 1. **User Pool = Secure Auth Database + Managed Auth API**

* Stores: `email`, `password`, `MFA`, `phone`, **custom fields** like `college`, `role`
* Gives: **JWT tokens** (`id_token`, `access_token`, `refresh_token`)
* Built-in endpoints: `/signup`, `/login`, `/forgot-password`, `/token`, etc.
* **Scales globally**, supports **MFA**, **email/OTP**, **social login** (Google, Facebook, Apple)
* Use **id\_token** to identify user (frontend), **access\_token** to secure your APIs (backend)

✅ Real Use:

* Frontend: User signs up/login via hosted UI or SDK
* Backend: Validate `access_token` → get user ID/email from JWT
* Example: React + Spring Boot → secure `/api/user/me` using JWT

---

## 🔹 2. **Identity Pool = AWS Access on Behalf of Users**

* Takes: JWT from User Pool (or Google, Facebook, etc.)
* Gives: **temporary IAM credentials** (STS)
* Lets you:
  🔸 Upload to S3 directly from browser
  🔸 Query DynamoDB
  🔸 Call Lambda securely
* Granular: You define what logged-in users can do with IAM policies

✅ Real Use:

* After login, get temp creds → use SDK to upload avatar to S3
* Guests? → Allow **unauthenticated identities** too (limited access)

---

## 🔑 Real World Scenarios:

| Use Case                                 | What to Use                                                                        |
| ---------------------------------------- | ---------------------------------------------------------------------------------- |
| Login/signup, OTP, MFA                   | ✅ User Pool                                                                        |
| Google Login                             | ✅ User Pool w/ Federation                                                          |
| Upload profile pic to S3                 | ✅ Identity Pool (via JWT)                                                          |
| Protect Spring Boot APIs                 | ✅ Use `access_token` from User Pool                                                |
| Host frontend outside AWS (e.g., Vercel) | ✅ Still works                                                                      |
| Let user directly fetch S3 file          | ✅ Identity Pool + Pre-signed URLs (optional)                                       |
| Admin-only actions                       | ✅ Define custom roles via `custom:role` claim in JWT + IAM policy on Identity Pool |

---

## 🔥 What Cognito **does NOT do**:

* ❌ Doesn’t store full user profile(other datas than username,password) (you store it in other DB)
* ❌ Doesn’t support custom login UI without using SDK/API
* ❌ Doesn’t give you access to user passwords (hashed and managed)

---

## ⚖️ When **NOT** to use Cognito:

* You want total control over auth flow → Use **Keycloak** or **custom JWT**
* You’re building **enterprise SSO with strict identity governance** → Consider **Auth0**, **Okta**

---

## ✅ When to use Cognito:

> 🔥 When you're building apps that need **secure login**, **JWT tokens**, and access to **AWS services like S3/Lambda** — with minimal setup and max scalability.

---

Next AWS service? Let’s go all-in, short + real.
