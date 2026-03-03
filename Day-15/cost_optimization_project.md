

# Day 15 – AWS Cost Optimization using Lambda

## 🎯 Goal

Automate deletion of stale EBS snapshots using AWS Lambda to reduce unnecessary storage costs.

---

# 🖥 Understanding EC2 Volume & Snapshot

## 📦 What is an EBS Volume?

* When an EC2 instance is created, a storage disk is automatically attached.
* This disk is called an **EBS Volume**.
* It stores:

  * Operating System
  * Applications
  * Data

Think of it as the **hard disk of your cloud server**.

Even if EC2 is stopped, the volume still exists (unless manually deleted).

---

## 📸 What is a Snapshot?

* A snapshot is a **backup copy of an EBS volume** at a specific point in time.
* Used for:

  * Backup
  * Disaster Recovery
  * Cloning volumes
  * Creating AMIs

Snapshots are stored in AWS infrastructure and are charged per GB.

---

# ⚠️ Cost Problem Scenario

1. Developer creates EC2.
2. Volume gets created automatically.
3. Developer creates snapshot.
4. Developer deletes EC2.
5. Developer deletes volume.
6. Developer forgets to delete snapshot.

Now:

* Snapshot is not attached to any volume.
* Snapshot is not used.
* AWS is still charging for it.

This is called a **stale snapshot**.

---

# 🧠 Solution

Use **AWS Lambda** to:

1. Fetch all EBS snapshots.
2. Check if snapshot is stale.
3. Delete unused snapshots automatically.

---

# 🛠 Lambda Python Code

```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')

    # Get all EBS snapshots owned by this account
    response = ec2.describe_snapshots(OwnerIds=['self'])

    # Get all running EC2 instances
    instances_response = ec2.describe_instances(
        Filters=[{'Name': 'instance-state-name', 'Values': ['running']}]
    )

    active_instance_ids = set()

    for reservation in instances_response['Reservations']:
        for instance in reservation['Instances']:
            active_instance_ids.add(instance['InstanceId'])

    # Iterate through each snapshot
    for snapshot in response['Snapshots']:
        snapshot_id = snapshot['SnapshotId']
        volume_id = snapshot.get('VolumeId')

        # Case 1: Snapshot has no volume attached
        if not volume_id:
            ec2.delete_snapshot(SnapshotId=snapshot_id)
            print(f"Deleted EBS snapshot {snapshot_id} (No Volume Found).")

        else:
            try:
                volume_response = ec2.describe_volumes(VolumeIds=[volume_id])

                # If volume exists but not attached to any running instance
                if not volume_response['Volumes'][0]['Attachments']:
                    ec2.delete_snapshot(SnapshotId=snapshot_id)
                    print(f"Deleted EBS snapshot {snapshot_id} (Volume Not Attached).")

            except ec2.exceptions.ClientError as e:
                if e.response['Error']['Code'] == 'InvalidVolume.NotFound':
                    # Volume was deleted
                    ec2.delete_snapshot(SnapshotId=snapshot_id)
                    print(f"Deleted EBS snapshot {snapshot_id} (Volume Deleted).")
```

---

# 🚀 Practical Steps

## Step 1: Create EC2 Instance

* Go to EC2 → Launch Instance
* A volume will be created automatically.

## Step 2: Create Snapshot

* Go to EC2 → Volumes
* Select Volume
* Click Actions → Create Snapshot

## Step 3: Delete EC2 and Volume

* Terminate EC2
* Delete Volume
* Do NOT delete Snapshot

Now snapshot becomes stale.

---

# 🛠 Create Lambda Function

1. Login to AWS Console
2. Go to Lambda Service
3. Click **Create Function**
4. Choose **Author from Scratch**
5. Runtime → Python 3.x
6. Paste above code
7. Click **Deploy**

---

# 🧪 Testing the Lambda

1. Go to Test tab
2. Click Create Test Event
3. Give event name
4. Save
5. Click Test

Check logs in CloudWatch.

---

# 🔐 Required IAM Permissions

Lambda execution role must have this policy:

```json
{
  "Effect": "Allow",
  "Action": [
    "ec2:DescribeSnapshots",
    "ec2:DescribeInstances",
    "ec2:DescribeVolumes",
    "ec2:DeleteSnapshot"
  ],
  "Resource": "*"
}
```

Without this permission, Lambda cannot delete snapshots.

---

# ⏰ Optional (Production Level)

You can schedule this Lambda daily using:

* EventBridge Rule
* Cron expression (e.g., run every night)

---

# 🎯 Final Outcome

This automation:

* Prevents stale backups
* Reduces unnecessary storage cost
* Improves AWS hygiene
* Implements real-world FinOps practice

---
