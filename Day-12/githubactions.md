
# 🚀 GitHub Actions

GitHub Actions is GitHub’s built-in CI/CD platform.

It allows you to:

* Automate testing
* Build Docker images
* Deploy applications
* Run scripts
* Trigger workflows on Git events

And the biggest advantage?

✅ No external installation
✅ No Jenkins server
✅ No plugin maintenance
✅ Fully integrated with your repository

---

# 🧩 What Do You Mean by “Plugins”?

In tools like:

* Jenkins

You must install plugins for:

* Git integration
* Docker builds
* Kubernetes deployment
* Credentials management

Managing plugins = version conflicts + maintenance headache.

In GitHub Actions:

👉 You don’t install plugins.
👉 You use **Actions**.

An *Action* is a reusable automation unit maintained by:

* GitHub
* Docker
* Community

Example:

```yaml
uses: actions/checkout@v3
```

That’s like using a prebuilt automation block.

---

# 📁 Folder Structure

Inside your repo:

```
.github/
   workflows/
       my-workflow.yml
```

Important:

* Folder must be exactly `.github/workflows`
* Files must be `.yml` or `.yaml`

GitHub automatically detects them.

---

# 🧠 Example 1: Python CI Pipeline

```yaml
name: My First GitHub Actions

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: [3.8, 3.9]

    steps:
    - uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest

    - name: Run tests
      run: |
        cd src
        python -m pytest addition.py
```

---

## 🔎 Let’s Break This Down

### 🔹 name:

```yaml
name: My First GitHub Actions
```

Just the pipeline name shown in GitHub UI.

---

### 🔹 on:

```yaml
on: [push]
```

Trigger condition.

Means:
👉 Run this workflow whenever someone pushes code.

Other triggers:

* pull_request
* schedule
* workflow_dispatch (manual)

---

### 🔹 jobs:

A workflow can have multiple jobs.

Here:

```yaml
jobs:
  build:
```

`build` is job name.

---

### 🔹 runs-on:

```yaml
runs-on: ubuntu-latest
```

This tells GitHub:

👉 Start a virtual machine
👉 Install Ubuntu
👉 Run steps there

GitHub provides free runners.

---

### 🔹 strategy + matrix

```yaml
strategy:
  matrix:
    python-version: [3.8, 3.9]
```

This is powerful.

It runs the same job twice:

* Once with Python 3.8
* Once with Python 3.9

Used for multi-version testing.

---

### 🔹 steps

Steps execute sequentially.

---

### 🔹 uses:

```yaml
uses: actions/checkout@v3
```

This downloads your repository into the runner.

Without this → no code → nothing works.

---

### 🔹 with:

Passes parameters to actions.

```yaml
with:
  python-version: ${{ matrix.python-version }}
```

`${{ }}` = GitHub expression syntax.

---

### 🔹 run:

Runs shell commands inside the runner.

---

# 🔥 Example 2: Deploy to Kubernetes

```yaml
name: Deploy to k8s

on:
  push:
    branches:
      - main

env:
  KUBECONFIG: ${{ secrets.KUBECONFIG }}

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Create Docker Image
      uses: docker/build-push-action@v2
      with:
        context: .
        push: true

    - name: Deploy to k8s
      uses: appleboy/kubectl-action@v1.0.0
      with:
        args: apply -f deployment.yaml
```

---

## 🧠 What’s Happening Here?

### Trigger:

Only when pushing to `main` branch.

---

### env:

```yaml
env:
  KUBECONFIG: ${{ secrets.KUBECONFIG }}
```

GitHub Secrets store sensitive data securely.

You never hardcode:

* Tokens
* Passwords
* Kubeconfig

Secrets are encrypted.

---

### Docker Build

This uses official Docker action:

```
docker/build-push-action
```

It:

* Builds Docker image
* Pushes to registry

---

### Kubernetes Deployment

Uses:

```
appleboy/kubectl-action
```

It runs:

```bash
kubectl apply -f deployment.yaml
```

Inside GitHub runner.

---

# 🔥 CI/CD Flow Here

```text
Developer Push
      ↓
GitHub Trigger
      ↓
Run Tests
      ↓
Build Docker Image
      ↓
Push Image
      ↓
Deploy to Kubernetes
```

Fully automated.

No server needed.

---

# 🆚 Why GitHub Actions > AWS CI/CD (For Most Teams)

| Feature           | GitHub Actions | AWS CI/CD             |
| ----------------- | -------------- | --------------------- |
| Setup             | Instant        | Multiple services     |
| Git Integration   | Native         | Separate (CodeCommit) |
| Community         | Huge           | Limited               |
| Plugin Management | None           | Required              |
| Startup Friendly  | Yes            | Rare                  |

