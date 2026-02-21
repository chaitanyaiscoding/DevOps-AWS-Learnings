
# 🌍 Day 6 – Amazon Route 53 & DNS Deep Dive

> From VPC Networking → To Global Internet Routing

---

# 🚀 What is DNS?

## 🧠 DNS = Domain Name System

DNS is like the **phonebook of the internet**.

Humans remember:

```
google.com
amazon.com
myapp.com
```

But computers communicate using:

```
142.250.182.14
54.239.28.85
```

So DNS translates:

```
Domain Name → IP Address
```

Example:

```
www.example.com → 3.108.24.11
```

Without DNS, we would need to remember IP addresses for every website.

---

## 📦 How DNS Works (Step-by-Step Flow)

When you type in your browser:

```
www.mywebsite.com
```

Here’s what happens:

1. Browser checks local cache
2. OS checks DNS cache
3. Request goes to ISP DNS Resolver
4. Resolver queries:

   * Root DNS
   * TLD DNS (.com, .org, etc.)
   * Authoritative DNS Server
5. IP Address is returned
6. Browser connects to that IP

---

## 🌐 What is Amazon Route 53?

**Amazon Route 53** is:

> A highly available and scalable cloud Domain Name System (DNS) web service provided by AWS.

It helps you:

* Register domain names
* Route internet traffic
* Perform health checks
* Automatically fail over applications

---

## 🔎 Why is it called Route 53?

* “Route” → Traffic routing
* “53” → DNS uses port 53

---

# 🏗 What Does Route 53 Provide?

## 1️⃣ Domain Registration

You can buy domains directly from AWS:

Example:

```
mycompany.com
devopslearning.site
```

---

## 2️⃣ DNS Management (Hosted Zones)

Route 53 stores DNS records inside something called:

> **Hosted Zone**

There are two types:

### 🔹 Public Hosted Zone

* Used for internet-facing domains
* Accessible globally

### 🔹 Private Hosted Zone

* Used inside VPC
* Not accessible from internet

---

## 3️⃣ Routing Policies (Powerful Feature 🔥)

Route 53 supports multiple routing strategies:

| Routing Type  | Purpose                    |
| ------------- | -------------------------- |
| Simple        | One resource               |
| Weighted      | Traffic split              |
| Latency-based | Lowest latency region      |
| Failover      | Active-passive setup       |
| Geolocation   | Based on user location     |
| Multi-value   | Multiple healthy resources |

This is very important in real DevOps architecture.

---

# 🛠 Practical Architecture Example

```
User
  ↓
Route 53
  ↓
Load Balancer
  ↓
EC2 Instances
```

Instead of accessing:

```
http://13.239.240.170
```

You access:

```
http://myapp.com
```

Route 53 resolves it to your Load Balancer or EC2 public IP.

---

