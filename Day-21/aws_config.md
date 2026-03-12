
## What is AWS Config?

AWS Config is an AWS service used to **monitor, record, and evaluate the configuration of AWS resources**.

It continuously checks whether your infrastructure **follows predefined rules and compliance policies**.

In simple terms:

> AWS Config helps organizations ensure that their cloud resources follow security and operational standards.

---

# Why AWS Config is Important

In large organizations there may be:

* Hundreds of EC2 instances
* Multiple VPCs
* Many databases
* Different teams creating resources

Manually checking whether all resources follow company rules is **impossible**.

AWS Config automatically tracks and evaluates these configurations.

---

# Example Scenario

Imagine an organization has a rule:

> Every EC2 instance must have **monitoring enabled**.

Service involved:
Amazon EC2

Now suppose a developer launches a new EC2 instance but **forgets to enable monitoring**.

Without AWS Config:

* No one immediately notices the mistake.

With AWS Config:

* The instance will be marked **Non-Compliant**.

AWS Config will show:

```text
Compliant Resources: 1
Non-Compliant Resources: 1
```

This helps the organization quickly detect violations.

---

# What AWS Config Tracks

AWS Config records:

* Resource creation
* Resource modification
* Resource deletion
* Configuration history

It also provides **compliance reports**.

---

# AWS Config Rules

AWS Config works using **rules**.

Rules define **what configuration is considered compliant**.

Example rules:

* EC2 must have monitoring enabled
* S3 buckets should not be public
* Security groups should not allow open SSH
* EBS volumes must be encrypted

Rules can be:

1. **AWS Managed Rules**
2. **Custom Rules**

---

# Custom Rules Using Lambda

Sometimes organizations need **custom validation logic**.

In that case we use:

AWS Lambda

Lambda will evaluate the resources and decide whether they follow the rule.

---

# Practical – Creating a Custom Config Rule

### Step 1 – Open AWS Config

Go to AWS Console and search for **Config**.

---

### Step 2 – Create a Rule

Click **Add Rule**.

Choose:

```text
Use Custom Lambda Function
```

---

### Step 3 – Provide Rule Details

Provide:

* Rule Name
* Description
* Lambda Function ARN

The Lambda function will contain logic to evaluate compliance.

---

# Trigger Types

AWS Config rules can run in two ways:

### 1. Configuration Change Trigger

Runs whenever a resource changes.

Example:

```text
EC2 instance created
EC2 instance modified
```

---

### 2. Periodic Trigger

Runs at a fixed interval.

Example:

```text
Every 1 hour
Every 24 hours
```

This works like a **cron job**.

---

# Lambda Logic

The Lambda function should:

1. Fetch all EC2 instances
2. Check if monitoring is enabled
3. Mark resource as compliant or non-compliant

Example logic:

```python
if monitoring_enabled:
    status = "COMPLIANT"
else:
    status = "NON_COMPLIANT"
```

---

# Important: Lambda Permissions

The Lambda function must have IAM permissions to:

* Describe EC2 instances
* Read monitoring status
* Report compliance

Service used:

AWS Identity and Access Management

Without proper permissions, the rule will fail.

---

# Architecture Flow

```text
EC2 Instance Created
        ↓
AWS Config Rule Triggered
        ↓
Lambda Function Executes
        ↓
Checks Monitoring Status
        ↓
Marks Resource:
Compliant / Non-Compliant
```

---

# Benefits of AWS Config

* Continuous compliance monitoring
* Security governance
* Resource configuration history
* Automated auditing
* Integration with Lambda automation

---

# Summary

AWS Config helps organizations:

* Track resource configurations
* Detect rule violations
* Maintain compliance
* Automate security monitoring

It is widely used in **enterprise cloud governance and security auditing**.
