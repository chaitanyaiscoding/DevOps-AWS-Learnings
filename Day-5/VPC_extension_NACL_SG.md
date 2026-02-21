
# 🚀 Day 5 – Security Groups & NACL (Deep Dive into VPC Security)

> Extension of Day 4 – VPC Architecture & Networking

---

# 🏗 Revisiting VPC Architecture (Full Picture)

Before understanding Security Groups and NACL, we must clearly understand the structure of a VPC.

## Typical Production Architecture

```
Internet
   │
Internet Gateway
   │
Public Subnet
   │   └── Load Balancer
   │
Route Table
   │
Private Subnet
   │   ├── Application Servers (EC2)
   │   └── Database
```

---

## 🌐 What is a Public Subnet?

A **Public Subnet** is a subnet that:

* Has a route to the Internet Gateway
* Can assign public IP addresses to instances
* Allows internet communication

How?

Route Table contains:

```
0.0.0.0/0 → Internet Gateway
```

That’s what makes it “public”.

---

## 🔐 What is a Private Subnet?

A **Private Subnet**:

* Does NOT have direct route to Internet Gateway
* Does not expose instances to internet
* Usually used for:

  * Application servers
  * Databases
  * Internal services

If internet access is required:
👉 It uses a **NAT Gateway**

---

# 🔥 Now Comes Security

AWS gives us **two layers of security** inside VPC:

1. **Security Groups (SG)** → Instance level
2. **NACL (Network Access Control List)** → Subnet level

Think of it like:

* 🏠 Security Guard at each house → Security Group
* 🚪 Security at colony gate → NACL

---

# 🛡 What is a Security Group (SG)?

Security Group is:

> A virtual firewall attached to EC2 instance.

It controls:

* Inbound traffic (incoming)
* Outbound traffic (outgoing)

---

## 📥 What is Inbound Traffic?

Traffic coming INTO your instance.

Example:

* Browser → EC2
* SSH → EC2
* HTTP → EC2

---

## 📤 What is Outbound Traffic?

Traffic going OUT from your instance.

Example:

* EC2 downloading updates
* EC2 calling external APIs

---

## ⚡ Important Characteristics of Security Groups

* Applied at **instance level**
* Stateful
* Only Allow rules (no explicit deny)
* If inbound is allowed → response automatically allowed

---

# 🚧 What is NACL?

NACL = **Network Access Control List**

It is:

> A firewall applied at the subnet level.

It controls:

* Entire subnet traffic
* Both inbound and outbound

---

## ⚡ Important Characteristics of NACL

* Applied at **subnet level**
* Stateless
* Supports Allow AND Deny rules
* Rules processed in number order (lowest first)

---

# 🎯 Primary Difference Between SG and NACL

| Feature          | Security Group      | NACL                       |
| ---------------- | ------------------- | -------------------------- |
| Level            | Instance            | Subnet                     |
| Stateful?        | Yes                 | No                         |
| Supports Deny?   | No                  | Yes                        |
| Rule Evaluation  | All rules evaluated | Rule number order          |
| Default Behavior | Deny all inbound    | Allow all (by default VPC) |

---

# 🧪 Practical Implementation (Hands-On)

## Step 1: Create VPC

* Go to VPC service
* Select **VPC and more**
* Choose CIDR → `10.0.0.0/16`
* AWS auto-creates:

  * Public subnets
  * Private subnets
  * Route tables
  * Internet Gateway

---

## Step 2: Launch EC2 in Public Subnet

Important configuration:

* Select created VPC
* Select Public Subnet
* Enable auto-assign public IP

---

## Step 3: Install Python HTTP Server

SSH into EC2:

```bash
sudo apt update
python3 -m http.server 8000
```

This starts HTTP server on port 8000.

---

## Step 4: Access from Browser

Open:

```
http://<public-ip>:8000
```

Result:
❌ Site can’t be reached

Why?

Because **Security Group does not allow port 8000 inbound**.

---

## Step 5: Fix Using Security Group

Add Inbound Rule:

* Type: Custom TCP
* Port: 8000
* Source: 0.0.0.0/0

Now:

✅ Application works.

---

# 🔎 Now Testing NACL Behavior

By default, NACL:

* Allows all inbound
* Allows all outbound

So it didn’t block traffic earlier.

---

## Step 6: Modify NACL

Add rule:

* Rule number: 99
* Port: 8000
* Source: 0.0.0.0/0
* Action: DENY

Now try again:

```
http://<public-ip>:8000
```

Result:
❌ Site can't be reached again

Even though:

* Security Group allows 8000

Because:

👉 NACL blocks traffic at subnet level BEFORE reaching instance.

---

# 🧠 Deep Understanding: Traffic Flow Order

Traffic enters VPC:

1. Internet Gateway
2. NACL (Subnet level)
3. Security Group (Instance level)
4. EC2 Instance

If NACL denies:
Traffic never reaches Security Group.

---

# 💼 Real-World DevOps Insight

In production:

* Security Groups → Fine-grained instance control
* NACL → Extra security layer for subnet segmentation
* Databases → Private subnet + restricted SG
* Load balancer → Public subnet + HTTP/HTTPS allowed

---

# ⚠ Common Mistakes Beginners Make

* Opening all traffic (0.0.0.0/0 for everything)
* Forgetting outbound rules in NACL
* Not understanding stateful vs stateless
* Confusing SG and NACL roles

---

# 🚀 How This Connects to DevOps

In DevOps:

* You must design secure architectures
* You must troubleshoot connectivity
* You must understand layered security

Today you:

* Built infra
* Deployed app
* Debugged networking
* Tested firewall layers

This is real DevOps skill building.

---
