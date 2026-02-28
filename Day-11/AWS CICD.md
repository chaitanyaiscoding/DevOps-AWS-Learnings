
# 🚀 Day 11 – CI/CD (Continuous Integration & Continuous Deployment)

> Automating the Software Delivery Pipeline

---

# 🧠 What is CI/CD?

CI/CD stands for:

* **CI** → Continuous Integration
* **CD** → Continuous Delivery / Continuous Deployment

CI/CD is:

> A practice that automates the process of building, testing, and deploying applications.

Instead of manually:

* Testing code
* Building artifacts
* Deploying to servers

Everything happens automatically when code changes.

---

# 🔁 What is Continuous Integration (CI)?

Continuous Integration means:

> Every time a developer commits code, it is automatically tested and built.

Flow:

```id="ci1"
Developer → Push Code → Run Tests → Build Application
```

Purpose:

* Catch bugs early
* Prevent broken builds
* Maintain code quality

---

# 🚀 What is Continuous Delivery (CD)?

Continuous Delivery means:

> After build and tests pass, the application is automatically prepared for deployment.

Deployment may require approval.

---

# ⚡ What is Continuous Deployment?

Continuous Deployment means:

> After successful build and tests, application is automatically deployed without manual approval.

Fully automated release.

---

# 🧰 Different CI/CD Tools

There are many CI/CD tools in the industry:

* Jenkins
* GitHub Actions
* GitLab CI/CD
* CircleCI
* Azure DevOps
* AWS CI/CD services

Today → We focus on AWS CI/CD.

---

# 🔎 Understanding CI/CD Using Jenkins Example

Let’s understand using **Jenkins**.

Suppose:

* Application code is stored in GitHub.
* We want to deploy it to Kubernetes (K8s).
* Jenkins is acting as orchestrator.

---

## 🔁 Complete Flow (Jenkins Example)

```id="jenkins-flow"
Developer → GitHub → Jenkins Pipeline → Build → Docker Image → Push Image → Deploy to Kubernetes
```

---

## Step 1 – Developer Pushes Code

When someone commits code:

* Webhook triggers Jenkins pipeline.
* Jenkins starts automated workflow.

---

## Step 2 – Run Tests

Jenkins executes:

* Unit tests
* Integration tests
* Lint checks
* Code quality scans

If tests fail → Pipeline stops ❌
If tests pass → Continue ✅

---

## Step 3 – Build Process (Very Important 🔥)

What happens in build stage?

Build stage:

* Compiles code (if needed)
* Installs dependencies
* Packages application
* Creates artifact

If using containers:

👉 A Docker image is built.

Example:

```id="build-stage"
docker build -t myapp:v1 .
```

Now we have:

Application packaged inside container image.

---

## Step 4 – Push Docker Image

Image is pushed to container registry:

* Docker Hub
* Amazon ECR
* Any private registry

Example:

```id="push-stage"
docker push myrepo/myapp:v1
```

---

## Step 5 – Deploy to Kubernetes

Kubernetes deployment is updated:

* New image version
* Rolling update begins
* Old pods replaced with new ones

Application is now live.

That is CI/CD in action.

---

# ☁️ AWS CI/CD Services

AWS provides managed services for CI/CD.

Instead of Jenkins, AWS offers:

* CodeCommit
* CodePipeline
* CodeBuild
* CodeDeploy

Together, they form AWS CI/CD ecosystem.

---

# 🔄 AWS CI/CD Flow

```id="aws-flow"
CodeCommit → CodePipeline → CodeBuild → CodeDeploy → Application
```

---

# 📦 What is AWS CodeCommit?

## 🔹 Definition

**AWS CodeCommit** is:

> A fully managed source control service hosted by AWS.

It is similar to:

* GitHub
* GitLab
* Bitbucket

But fully inside AWS.

---

## 🔹 What Does CodeCommit Do?

It stores:

* Source code
* Branches
* Pull requests
* Version history

It supports:

* Git commands
* Secure access
* IAM integration

---

## 🔹 Why Use CodeCommit?

* No external GitHub dependency
* Tight AWS integration
* IAM-based access control
* Encrypted at rest & in transit

Especially useful for:

* Enterprises
* Internal projects
* Private repositories

---

# 🔁 What is AWS CodePipeline?

CodePipeline is:

> The orchestrator of AWS CI/CD.

It connects all stages:

* Source
* Build
* Test
* Deploy

It automates the workflow.

Equivalent of Jenkins pipeline.

---

# 🔨 What is AWS CodeBuild?

CodeBuild is:

> Fully managed build service.

It:

* Compiles source code
* Runs tests
* Builds Docker images
* Produces artifacts

Equivalent of Jenkins build stage.

No need to manage build servers.

---

# 🚀 What is AWS CodeDeploy?

CodeDeploy is:

> Deployment service that pushes application to:

* EC2
* On-premise servers
* Lambda
* ECS

It supports:

* Rolling deployments
* Blue/Green deployments
* Automated rollback

Equivalent of deployment stage in Jenkins.

---

# 🧠 Comparing Jenkins vs AWS CI/CD

| Jenkins            | AWS CI/CD Equivalent |
| ------------------ | -------------------- |
| GitHub             | CodeCommit           |
| Jenkins Pipeline   | CodePipeline         |
| Jenkins Build      | CodeBuild            |
| Deployment Scripts | CodeDeploy           |

Difference:

* Jenkins → Self-managed
* AWS CI/CD → Fully managed

---

# 🧠 When To Use AWS CI/CD?

Use AWS CI/CD when:

* Entire infrastructure is in AWS
* You want managed services
* You don’t want to manage Jenkins servers
* You want IAM integration

---

# 🔥 Real DevOps Understanding

CI/CD is not about tools.

It is about:

* Automation
* Speed
* Reliability
* Faster feedback
* Continuous delivery of value

---

# 🧠 Big Picture Architecture (AWS CI/CD)

```id="full-aws-cicd"
Developer
   ↓
CodeCommit
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
CodeDeploy
   ↓
EC2 / ECS / Lambda / K8s
```

Every commit → Automatically tested → Built → Deployed.

