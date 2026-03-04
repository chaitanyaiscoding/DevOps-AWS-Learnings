
# 🌍 AWS CloudFront

## What is CloudFront?

Amazon CloudFront is AWS’s **Content Delivery Network (CDN)** service.

It helps deliver:

* Images
* Videos
* Static websites
* APIs
* Dynamic content

to users with **low latency and high speed**.

---

# 🌐 What is a CDN?

CDN = **Content Delivery Network**

It is a **network of global servers (edge locations)** that cache content closer to users.

---

## 🧠 Simple Example to Understand CDN

Imagine:

* Your website is hosted in **Mumbai region**.
* A user opens your site from **USA**.

Without CDN:

```text
User (USA) → Request → Mumbai Server → Response → USA
```

This takes time (high latency).

---

With CDN:

```text
User (USA) → Nearest Edge Location (USA) → Response
```

The CDN already has a **cached copy** of your content.

So response is much faster.

---

# 🚀 What Does CDN Actually Do?

✔ Caches static content
✔ Reduces latency
✔ Reduces load on origin server
✔ Improves user experience
✔ Protects backend from traffic spikes

---

# 🔎 Important Point You Mentioned

> CDN is placed before the request goes to main server

Correct ✅

Architecture looks like this:

```text
User → CloudFront (CDN) → S3 / EC2 (Origin)
```

CloudFront sits in front of your backend.

---

# 🌎 How Does CloudFront Work?

1. User requests website.
2. DNS maps domain to CloudFront.
3. CloudFront checks nearest Edge Location.
4. If content is cached → return immediately.
5. If not cached → fetch from origin (S3).
6. Store copy in edge location.
7. Serve future users faster.

---

# 🏗 Practical – Hosting Static Website with CloudFront

## Step 1️⃣ Host Static Website in S3

Go to:

Amazon S3

1. Create S3 bucket.
2. Upload:

   * index.html
   * CSS
   * Images
3. Enable static website hosting.
4. Make objects public (or use proper policy).

Now your site is hosted — but only in one region.

---

## Step 2️⃣ Go to CloudFront

Search for:

CloudFront Service

Click:

Create Distribution

---

## Step 3️⃣ Configure Distribution

### Origin

* Origin Domain → Select your S3 bucket
* CloudFront will fetch content from this bucket

---

### Origin Access Identity (OAI)

Create Access Identity.

This allows:

* CloudFront to access private S3 content
* Users cannot directly access S3
* They must go through CloudFront

This improves security.

---

### Edge Locations

CloudFront automatically uses:

✔ All global edge locations

You don’t manually select them — AWS handles routing.

---

## Step 4️⃣ Create Distribution

Click:

Create Distribution

Wait 5–10 minutes for status:

```text
In Progress → Deployed
```

---

# 🌐 Accessing Website via CloudFront

After deployment:

You will get a domain like:

```text
d123abcd.cloudfront.net
```

Open it in browser.

Now your website is served via CDN.

---

# 🧠 Why Not Directly Use S3 Website URL?

Because:

| S3 Only        | CloudFront                 |
| -------------- | -------------------------- |
| Single region  | Global edge network        |
| Higher latency | Low latency                |
| No caching     | Intelligent caching        |
| Less secure    | Can restrict direct access |

---

# 🎯 Real Architecture

```text
User
   ↓
DNS
   ↓
CloudFront (CDN - Edge Location)
   ↓
S3 Bucket (Origin - Single Region)
```
