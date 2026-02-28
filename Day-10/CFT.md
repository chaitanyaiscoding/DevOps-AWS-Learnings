
# 🚀 Day 10 – CloudFormation (CFT) & Infrastructure as Code (IaC)

> From Commands → To Declarative Infrastructure

---

# 🧠 Problem with AWS CLI

AWS CLI is powerful.

But…

When creating complex resources like EC2, the command becomes:

* Very long
* Hard to remember
* Error-prone
* Difficult to manage dependencies

Example:

Creating EC2 via CLI requires:

* AMI ID
* Subnet ID
* Security Group ID
* Key pair
* IAM role
* User data
* Tags

The command becomes huge.

If you need to modify something later → you must manually adjust commands again.

This is not scalable for large environments.

---

# 💡 Solution → Infrastructure as Code (IaC)

Instead of writing long CLI commands…

We write infrastructure in **code format**.

This concept is called:

# 🏗 What is Infrastructure as Code (IaC)?

Infrastructure as Code means:

> Writing code to create and manage infrastructure.

Instead of:

Clicking UI ❌
Running long CLI commands ❌

We write:

```yaml
Resources:
  MyEC2:
    Type: AWS::EC2::Instance
```

And the infrastructure is created automatically.

---

# 📌 Core Principle of IaC

Any IaC tool must:

> Act as a middle layer between User and Cloud

Flow:

```id="tfr6ya"
User → IaC Tool → Cloud API → Cloud Resources
```

User does NOT directly call API.

The IaC tool converts code → API calls.

---

# 🧰 Tools Based on IaC Principles

IaC is a **concept**.

Tools built using this concept:

* AWS CloudFormation (CFT)
* Terraform
* AWS CDK

Important:

* CloudFormation supports ONLY AWS
* Terraform supports multi-cloud

---

# 🌩 What is AWS CloudFormation (CFT)?

**AWS CloudFormation** is AWS’s native IaC service.

It allows you to:

* Write infrastructure in YAML or JSON
* Submit template
* Automatically create resources

Behind the scenes:

```id="u2d9sk"
YAML Template → Converted to API Calls → AWS Resources Created
```

Eventually, an API call is always made.

---

# 🟡 Why YAML Is Preferred Over JSON?

YAML is:

* Cleaner
* More readable
* Less syntax heavy
* Easier to maintain

That’s why YAML is commonly used in CloudFormation.

---

# 📦 What Is a Stack?

In CloudFormation:

> A Stack is a collection of AWS resources created from a template.

You don’t directly upload YAML and create resources.

Instead:

1. Create a Stack
2. Upload Template
3. CloudFormation processes it
4. Resources are created

Stack = Running instance of your template.

---

# 🧠 What Do We Write in a CloudFormation Template?

A template contains structured sections:

---

## 1️⃣ Version

Defines template format version.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
```

---

## 2️⃣ Description

Explains what this template does.

```yaml
Description: Create EC2 instance with security group
```

---

## 3️⃣ Metadata

Optional information about template.

---

## 4️⃣ Parameters

Used to pass dynamic values.

Example:

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
```

Instead of hardcoding values, we pass parameters.

---

## 5️⃣ Rules

Validation rules for parameters.

---

## 6️⃣ Mappings

Used to define region-based or condition-based values.

Example:

```yaml
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-123456
```

---

## 7️⃣ Resources (Most Important Section 🔥)

This defines:

> What exactly we want to create.

Example:

```yaml
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
```

Without Resources section → Nothing gets created.

---

# 🔎 What is Drift Detection?

Drift happens when:

* Someone manually modifies infrastructure
* Outside CloudFormation

Example:

You created EC2 using CFT.
Then someone changes security group via UI.

Now:

Template ≠ Actual Infrastructure

CloudFormation Drift Detection:

* Compares template
* Compares live resources
* Detects mismatches

This ensures configuration consistency.

---

# ⚖️ When to Use CLI vs CloudFormation?

---

## ✅ Use AWS CLI When:

* Quick testing
* Running single commands
* Debugging
* Learning services
* Scripting small tasks

---

## ✅ Use CloudFormation When:

* Creating production infrastructure
* Managing multiple resources together
* Version controlling infrastructure
* Reproducible environments
* Team collaboration
* Infrastructure standardization

In production → IaC is preferred.

---

# 🛠 Practical – Creating Resources Using CloudFormation

---

## Step 1 – Open CloudFormation

Go to:

AWS Console → CloudFormation

---

## Step 2 – Create Stack

Click:

👉 Create Stack

This is where we submit our template.

---

## Step 3 – Create Template

You can:

* Upload YAML file
* Use CloudFormation Designer
* Drag & Drop resources visually

Designer allows you to:

* Drag EC2
* Add Security Group
* Add VPC
* Auto-generate template

---

## Step 4 – Use Existing Template

After creating template:

Go back → CloudFormation
Choose:

👉 Use Existing Template

Upload your YAML file.

---

## Step 5 – Name the Stack

Give a stack name.

Click Submit.

---

# 🚀 What Happens Next?

CloudFormation:

1. Reads YAML
2. Converts it to API calls
3. Calls respective AWS services
4. Creates resources
5. Tracks them under stack

Boom 💥
Infrastructure created.

---

# 🧠 Big Picture Understanding

CLI = Imperative approach
(You tell step-by-step what to do)

CloudFormation = Declarative approach
(You declare what you want, AWS figures out how)

Example:

CLI:

```id="a2m9fz"
Create EC2 with this AMI, this subnet, this SG...
```

CFT:

```id="kq4p7e"
I want an EC2 instance.
```

AWS handles the rest.


