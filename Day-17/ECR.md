
# Day 17 – Amazon ECR

## What is ECR?

Amazon Elastic Container Registry (ECR) is an AWS service used to **store, manage, and deploy container images**.

ECR stands for:

**Elastic Container Registry**

It is mainly used to store **Docker container images** which can later be pulled by services like:

* Amazon Elastic Container Service
* Amazon Elastic Kubernetes Service
* EC2 instances
* CI/CD pipelines

---

# Why Do We Need ECR if Docker Hub Already Exists?

Docker Hub is a public container registry.

However, ECR provides several advantages inside AWS.

## Key Differences

| Docker Hub         | ECR                         |
| ------------------ | --------------------------- |
| Public by default  | Private by default          |
| External service   | Fully integrated with AWS   |
| Limited free pulls | Optimized for AWS workloads |
| Basic security     | IAM based access control    |

---

# Advantages of ECR

### 1️⃣ Private Repository by Default

Images stored in ECR are **private**, making them secure for enterprise use.

---

### 2️⃣ IAM Integration

ECR integrates with:

* AWS Identity and Access Management

This allows fine-grained access control for:

* Developers
* CI/CD systems
* Kubernetes clusters

---

### 3️⃣ Built-in Image Scanning

ECR can **scan container images for vulnerabilities** automatically.

This helps identify:

* Security risks
* Outdated libraries
* CVEs

---

### 4️⃣ Seamless AWS Integration

ECR works smoothly with:

* Amazon Elastic Kubernetes Service
* Amazon Elastic Container Service
* AWS CodeBuild
* AWS CodePipeline

---

### 5️⃣ Faster Pulls in AWS Network

When running containers inside AWS, pulling images from ECR is **faster than external registries**.

---

# Practical – Using ECR

## Step 1 – Open ECR Service

Go to AWS Console.

Search for:

**Elastic Container Registry**

---

## Step 2 – Create Repository

Click:

**Create Repository**

Choose:

* Repository Name
* Enable **Image Scanning**

Then click:

**Create Repository**

---

# Pushing Docker Image to ECR

AWS provides commands directly inside the **View Push Commands** section.

---

# Step 3 – Authenticate Docker to ECR

Run:

```bash
(Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin 948091636482.dkr.ecr.us-east-1.amazonaws.com
```

This command logs Docker into your ECR registry.

---

# Step 4 – Build Docker Image

```bash
docker build -t registry/ecr1 .
```

This creates a Docker image named:

```
registry/ecr1
```

---

# Step 5 – Tag the Docker Image

```bash
docker tag registry/ecr1:latest 948091636482.dkr.ecr.us-east-1.amazonaws.com/registry/ecr1:latest
```

Tagging connects your local image with the **ECR repository URL**.

---

# Step 6 – Push Image to ECR

```bash
docker push 948091636482.dkr.ecr.us-east-1.amazonaws.com/registry/ecr1:latest
```

Now the image will be uploaded to ECR.

You will see the image inside your repository.

---

# Architecture Flow

```
Developer → Docker Build → Push Image → ECR Repository
                                      ↓
                           ECS / EKS / EC2 pulls image
```

---

# Real World Usage

Typical DevOps workflow:

```
Developer → Git Push
          ↓
CI/CD Pipeline
          ↓
Docker Image Build
          ↓
Push to ECR
          ↓
Deploy to ECS / Kubernetes
```


