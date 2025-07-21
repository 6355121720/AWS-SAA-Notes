# 📘 AWS Route 53 & DNS - In-Depth Notes

---

## 🔷 Route 53 Overview

Amazon Route 53 is a scalable and highly available Domain Name System (DNS) web service. It performs:

* Domain registration
* DNS routing
* Health checking and failover

---

## 🔸 Routing Policies

| Policy                    | Description                                                                           |
| ------------------------- | ------------------------------------------------------------------------------------- |
| **Simple Routing**        | Routes traffic to a single resource (e.g., EC2, Elastic Beanstalk, S3).               |
| **Weighted Routing**      | Distributes traffic between multiple resources using percentage weights.              |
| **Latency-based Routing** | Sends traffic to the region with the lowest network latency from the user’s location. |
| **Geolocation Routing**   | Routes users based on their **country/continent**.                                    |
| **Geoproximity Routing**  | Routes based on user’s location and allows **bias** adjustments to shift traffic.     |
| **Failover Routing**      | Primary/Secondary failover setup; traffic sent to backup if primary fails.            |
| **Multivalue Answer**     | Returns multiple healthy IPs randomly to simulate simple load balancing.              |

---

## 🧾 DNS Record Types

| Record Type     | Purpose                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------- |
| **A Record**    | Maps domain → IPv4 address.                                                               |
| **AAAA Record** | Maps domain → IPv6 address.                                                               |
| **CNAME**       | Maps subdomain → another domain. Not allowed at root domain.                              |
| **ALIAS**       | Like CNAME but allowed at root (only in Route 53). Maps to AWS resources (e.g., ELB, S3). |
| **NS Record**   | Specifies which name servers manage the domain.                                           |

### TTL (Time To Live)

* Controls how long a DNS response is cached.
* **High TTL (e.g., 24h)**: Less DNS traffic, more stale data.
* **Low TTL (e.g., 60s)**: More frequent lookups, more accurate.

---

## 🌐 DNS Hierarchy

```
Root → TLD → SLD → Subdomain → Hostname
```

### Example: `blog.store.example.com`

| Level         | Part                     |
| ------------- | ------------------------ |
| Root          | (invisible)              |
| TLD           | `.com`                   |
| SLD           | `example`                |
| Subdomain     | `store`                  |
| Sub-subdomain | `blog`                   |
| FQDN          | `blog.store.example.com` |
| Hostname      | e.g., `www`, `mail`      |

---

## 🔁 DNS Resolution Flow

1. **Browser/OS Cache**: Checks if the IP is already cached.
2. **Local Resolver (ISP)**: Asks the ISP’s DNS resolver.
3. **Recursive Query Chain**:

   * Root DNS Server → directs to TLD (.com)
   * TLD Server → directs to Authoritative NS
   * Authoritative NS → returns actual IP
4. **Caching**: IP is cached by resolver and browser.
5. **Connection**: Browser uses returned IP to contact server.

---

## 📘 DNS Terminologies

| Term                  | Meaning                                                              |
| --------------------- | -------------------------------------------------------------------- |
| **Domain Registrar**  | Services like Route 53, GoDaddy, Namecheap to register domain names. |
| **Zone File**         | File that contains all the DNS records for a domain.                 |
| **Name Server (NS)**  | Server that responds to DNS queries for a domain.                    |
| **Authoritative DNS** | Final source of truth for domain records.                            |

---

## 🛠️ Health Checks in Route 53

* Can monitor EC2, ELB, or external endpoints.
* Integrated with failover routing policies.
* Sends HTTP/HTTPS or TCP requests to verify health.
* Can trigger CloudWatch alarms or SNS notifications.

---

## 🧭 Route 53 Domain Registration

* Supports domain registration directly in AWS.
* Provides default hosted zone with NS and SOA records.
* Auto-renewal, WHOIS privacy, and DNSSEC available.

---

