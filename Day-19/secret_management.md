

# Secrets Management in AWS

Today we are going to learn about **Secrets Management in AWS**.

## Why is Secrets Management Important?

In modern cloud applications, we often use **credentials, API keys, tokens, and passwords** to access different services. These are called **secrets**, and they must be protected.

For example, if we have a **container registry**, we must keep its credentials secure. If these credentials are leaked:

* Someone could **delete images**
* Replace images with **malicious versions**
* Push **unauthorized containers**
* Pull **private images**

This could break deployments or compromise the entire system.

---

## More Examples of Secrets

Here are some common secrets used in applications:

### 1. Database Credentials

Example:

* MySQL / PostgreSQL username & password

If leaked:

* Attackers can **read or delete data**
* Modify production databases

---

### 2. API Keys

Example:

* Payment gateways
* Third-party services

If exposed:

* Someone can **make API calls on your behalf**
* Can cause **unexpected billing**

---

### 3. Cloud Credentials

Example:

* AWS access key & secret key

If leaked:

* Someone can **create resources**
* Delete infrastructure
* Mine cryptocurrency using your account

---

### 4. Container Registry Credentials

Example:

* Credentials for a container registry like
  Amazon Elastic Container Registry

If leaked:

* Images can be deleted
* Images can be replaced with **malicious ones**

---

# Secrets Management Services

AWS provides different ways to manage secrets securely.

---

# 1. AWS Systems Manager Parameter Store

Service:
AWS Systems Manager

Parameter Store allows you to store:

* Configuration values
* Secrets
* Environment variables

### Features

* Store values as **encrypted parameters**
* Integrated with **IAM**
* Supports **versioning**
* Works well with **EC2, Lambda, ECS**

### Example Use Cases

Store:

```
/app/prod/db-password
/app/prod/api-key
/app/dev/config
```

Applications can fetch them securely.

### When to Use

Use **Parameter Store** when:

* You need **simple secret storage**
* Secrets do **not rotate often**
* You want **low cost**

---

# 2. AWS Secrets Manager

Service:
AWS Secrets Manager

This is a **fully managed secret storage service** designed specifically for credentials.

### Key Feature: Automatic Rotation

Secrets Manager can **automatically rotate credentials**.

Example:

* Rotate **database passwords**
* Rotate **API keys**

Rotation can be automated using **Lambda functions**.

### Features

* Automatic **secret rotation**
* Encryption using **AWS KMS**
* Fine-grained access control
* Integration with many AWS services

### Example Use Cases

* RDS database passwords
* API tokens
* OAuth credentials

### When to Use

Use **Secrets Manager** when:

* You need **automatic rotation**
* You store **sensitive credentials**
* You want **managed lifecycle of secrets**

---

# 3. HashiCorp Vault

Tool:
HashiCorp Vault

This is a **third-party secrets management tool** widely used in enterprise environments.

It is **not AWS native**, but can run on AWS.

### Key Features

* Dynamic secrets
* Secret leasing
* Encryption as a service
* Identity-based access

### Example

Vault can generate **temporary database credentials**:

```
username: app-user-123
password: random-pass
ttl: 1 hour
```

After 1 hour, the credentials automatically expire.

### When to Use

Use **Vault** when:

* You are **multi-cloud**
* You need **advanced secret workflows**
* Your company already uses **HashiCorp tools**

---

# When to Combine System Manager + Secrets Manager

Many companies use **both services together**.

### Example Architecture

Parameter Store → configuration values
Secrets Manager → sensitive secrets

Example:

Parameter Store:

```
/app/prod/db-host
/app/prod/db-port
/app/prod/env
```

Secrets Manager:

```
db-password
api-token
payment-key
```

### Why Combine Them?

Because:

* Parameter Store is **cheap and good for configs**
* Secrets Manager is **best for highly sensitive secrets**

So we split the responsibilities.

---

# Quick Comparison

| Feature        | Parameter Store | Secrets Manager | Vault        |
| -------------- | --------------- | --------------- | ------------ |
| Cost           | Low             | Higher          | Self-managed |
| Rotation       | Manual          | Automatic       | Dynamic      |
| AWS Native     | Yes             | Yes             | No           |
| Enterprise Use | Medium          | High            | Very High    |

