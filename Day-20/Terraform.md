Here is a **clean structured version of your Day-20 notes** with explanations and Terraform flow.

---

# Day 20 – Terraform (Infrastructure as Code)

Today we are going to learn about **Terraform** and build a small **AWS infrastructure project** using it.

Terraform is an **Infrastructure as Code (IaC)** tool used to create, manage, and destroy cloud infrastructure using code.

Tool:
Terraform

---

# Why Terraform when we already have CloudFormation?

AWS already provides a tool called:

AWS CloudFormation

But Terraform is widely used because of several advantages.

### 1. Multi-Cloud Support

CloudFormation works **only with AWS**.

Terraform works with:

* AWS
* Azure
* Google Cloud
* Kubernetes
* GitHub
* Many other platforms

So companies using **multi-cloud** prefer Terraform.

---

### 2. Simpler Syntax

Terraform uses **HCL (HashiCorp Configuration Language)** which is easier than large JSON/YAML templates.

Example:

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
```

---

### 3. Better State Management

Terraform maintains a **state file** that tracks:

* What infrastructure exists
* What needs to be created
* What needs to be destroyed

---

# Requirements

Before starting Terraform with AWS, we need:

### 1. Install Terraform

Install from official site:

[https://developer.hashicorp.com/terraform/downloads](https://developer.hashicorp.com/terraform/downloads)

---

### 2. AWS Credentials

Terraform needs credentials to create resources in AWS.

Service:
AWS Identity and Access Management

Create **Access Key and Secret Key** from:

AWS Console → Security Credentials → Access Keys

⚠️ Best practice:

Use **IAM user**, not **root user**.

Reason:

* Root user has full permissions
* Security risk if leaked

---

# Project Setup

### Step 1 – Create Project Folder

In **VS Code**

```bash
terraform-project/
```

---

# Terraform Files

### provider.tf

This file tells Terraform **which cloud provider we are using**.

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Terraform supports many providers:

* AWS
* Azure
* GCP
* Kubernetes
* GitHub

---

# Initialize Terraform

Run:

```bash
terraform init
```

What this does:

* Downloads provider plugins
* Creates `.terraform` working directory
* Links Terraform with AWS provider

---

# Practical Infrastructure Creation

Now we start creating resources.

---

# Step 1 – Create VPC

File: **main.tf**

```hcl
resource "aws_vpc" "main_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "my-vpc"
  }
}
```

Service:
Amazon Virtual Private Cloud

A **VPC** is a private network inside AWS where our resources run.

---

# Step 2 – Create Subnets

Subnets divide the VPC into smaller networks.

```hcl
resource "aws_subnet" "subnet1" {
  vpc_id     = aws_vpc.main_vpc.id
  cidr_block = "10.0.1.0/24"
}

resource "aws_subnet" "subnet2" {
  vpc_id     = aws_vpc.main_vpc.id
  cidr_block = "10.0.2.0/24"
}
```

---

# Step 3 – Create Internet Gateway

Service:
AWS Internet Gateway

This allows the VPC to communicate with the **internet**.

```hcl
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main_vpc.id
}
```

---

# Step 4 – Create Route Table

A route table defines **where network traffic goes**.

```hcl
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main_vpc.id
}
```

Add route to internet:

```hcl
resource "aws_route" "internet_access" {
  route_table_id         = aws_route_table.public_rt.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.igw.id
}
```

---

# Step 5 – Associate Route Table to Subnets

Now subnets can access the internet.

```hcl
resource "aws_route_table_association" "subnet1_association" {
  subnet_id      = aws_subnet.subnet1.id
  route_table_id = aws_route_table.public_rt.id
}

resource "aws_route_table_association" "subnet2_association" {
  subnet_id      = aws_subnet.subnet2.id
  route_table_id = aws_route_table.public_rt.id
}
```

---

# Terraform Commands

### Validate Code

```bash
terraform validate
```

Checks for syntax errors.

---

### Plan Infrastructure

```bash
terraform plan
```

Shows:

* What Terraform will create
* What changes will happen

Example:

```
+ create VPC
+ create subnet
+ create internet gateway
```

Nothing is created yet.

---

### Apply Infrastructure

```bash
terraform apply
```

Terraform now actually **creates the resources in AWS**.

---

# Additional Infrastructure (Optional)

After networking setup we can create:

### Security Groups

Service:
Amazon EC2 Security Groups

Used as firewall rules.

Example:

```hcl
resource "aws_security_group" "web_sg" {
  vpc_id = aws_vpc.main_vpc.id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

---

### Load Balancer

Service:
Elastic Load Balancing

Distributes traffic to multiple servers.

---

# What We Built

Using Terraform we created:

* VPC
* Subnets
* Internet Gateway
* Route Table
* Route Table Associations

This forms the **basic AWS networking architecture**.

---

# Important Terraform Concept

Terraform follows:

```
Write Code → Plan → Apply → Infrastructure Created
```

Everything becomes **version controlled infrastructure**.

---

✅ **Big advantage**

With one command you can also destroy everything:

```bash
terraform destroy
```

All resources will be removed automatically.

---

If you want, I can also show you **the exact Terraform project structure companies use (modules, variables, outputs)** which is **very important for DevOps interviews and real projects**.
