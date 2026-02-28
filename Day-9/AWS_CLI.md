

# 🚀 Day 9 – AWS CLI (Command Line Interface)

> From Manual Work to Automation

---

# 🧠 Why Do We Need AWS CLI?

Until now, we did everything using:

👉 **AWS Console (UI)**

But here is the problem:

* UI is manual
* Not automation-friendly
* Time-consuming
* Not scalable
* Error-prone

Example:

If your manager says:

> “Create 10 VPCs”

Using UI:

* Click → Create VPC
* Repeat 10 times manually ❌

That is not DevOps.

DevOps = Automation.

---

# 🤖 What Is Automation in AWS?

Automation means:

* Sending instructions programmatically
* Avoiding manual repetition
* Making infrastructure reproducible

AWS provides multiple automation tools:

* AWS CLI
* Terraform
* CloudFormation (CFT)
* AWS CDK
* SDK (for developers)

Today → We focus on **AWS CLI**.

---

# 🌐 What Is AWS CLI?

**AWS CLI (Command Line Interface)** is:

> A tool that allows you to interact with AWS services using commands in your terminal.

It works by calling AWS APIs behind the scenes.

---

# 🔍 What Is an API?

API = Application Programming Interface

Think of it like:

```id="lxr03u"
You → API → AWS Service → Response
```

Instead of clicking buttons:

You send a command like:

```bash
aws ec2 describe-instances
```

AWS API processes it and returns output.

---

# ⚙️ How AWS CLI Works Internally

AWS has already written:

* Functions
* Methods
* Service calls

These are abstracted (hidden complexity).

We just:

* Import those functions (via CLI)
* Pass parameters
* Execute commands

That’s it.

We don’t write low-level networking code.

AWS provides abstraction.

---

# 🐍 Is AWS CLI Written in Python?

Yes.

AWS CLI is:

* Built using Python
* Uses AWS SDK (Boto3 internally)

That’s why you need Python installed locally.

---

# 🛠 Installing AWS CLI (Practical)

### Step 1 – Install AWS CLI

Run in terminal (Windows):

```bash
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```

Silent installation:

```bash
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi /qn
```

---

### Step 2 – Verify Installation

Run:

```bash
aws --version
```

If installed correctly, you’ll see:

```id="8xj5re"
aws-cli/2.x.x Python/3.x Windows/...
```

If not working:

* Restart terminal
* Check environment variables

---

# 🐍 Why Do We Need Python?

AWS CLI uses Python runtime.

If Python is not installed:

* Some dependencies may fail
* SDK functionality may not work properly

Check Python:

```bash
python --version
```

---

# 🔐 Step 3 – Connect AWS CLI to Your AWS Account

Now we must authenticate.

Run:

```bash
aws configure
```

It will ask:

1. AWS Access Key ID
2. AWS Secret Access Key
3. Default Region
4. Default Output Format

---

## 🔑 Where Do We Get Access Keys?

Go to:

AWS Console →
IAM →
Users →
Security Credentials →
Create Access Key

⚠ Important:

* Never share secret key
* Never push it to GitHub
* Use IAM user with limited permissions

---

After entering credentials:

Boom 💥
You are connected to AWS.

---

# ✅ Test the Connection

Run a simple command:

```bash
aws s3 ls
```

This lists:

* All your S3 buckets

If buckets are listed → Configuration successful.

---

# 🧠 Understanding CLI Command Structure

Most AWS CLI commands follow pattern:

```id="z3p8aa"
aws <service> <operation> <parameters>
```

Examples:

List EC2 instances:

```bash
aws ec2 describe-instances
```

Create S3 bucket:

```bash
aws s3 mb s3://my-bucket-name
```

Delete bucket:

```bash
aws s3 rb s3://my-bucket-name
```

---

# 📘 Where Do We Find All Commands?

AWS provides:

👉 **AWS CLI Command Reference**

Every service has documentation with:

* Command syntax
* Required parameters
* Examples

You can also type:

```bash
aws help
aws ec2 help
```

---

# 🔥 Why AWS CLI Is Powerful

With CLI you can:

* Create VPCs
* Launch EC2
* Create S3 buckets
* Automate deployments
* Integrate with CI/CD
* Write shell scripts

Example automation:

```bash
for i in {1..10}
do
   aws ec2 create-vpc --cidr-block 10.$i.0.0/16
done
```

Now 10 VPCs are created automatically 🚀

Try doing that in UI 😄

---

# 🔒 Security Best Practices

* Use IAM user, not root
* Use least privilege access
* Rotate access keys
* Never store keys in public repos
* Use AWS CLI profiles for multiple accounts

---

# 🧠 DevOps Mindset Shift

UI → For learning
CLI → For automation
Terraform → For infrastructure as code
CI/CD → For continuous deployment

Real DevOps engineers rarely use UI in production.

---

### ❓ Does CLI directly control AWS?

No.

CLI sends request to:

AWS API → Service → Response returned.

---
