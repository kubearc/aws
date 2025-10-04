# AWS Lambda Practice: Start & Stop EC2 Instances (Console-Based)

## 🎯 Goal
Automate EC2 instance start and stop operations using **AWS Lambda + EventBridge (Cron jobs)** — via AWS Console only.

---

## 🧩 Part 1 — Prerequisites

### ✅ Requirements
1. **EC2 instance:** one active instance (record its Instance ID)
2. **IAM Role:** allows Lambda to manage EC2 and write logs

### Steps
#### 🖥️ Launch EC2 Instance
1. Go to **EC2 → Launch instances**
2. Name: `lambda-demo-instance`
3. AMI: Amazon Linux 2, Type: `t2.micro`
4. Configure key pair and defaults → Launch
5. Note your **Instance ID**, e.g., `i-0123456789abcdef0`

#### 🔐 Create IAM Role for Lambda
1. Go to **IAM → Roles → Create role**
2. Trusted entity: **AWS Service** → Use case: **Lambda**
3. Attach policies:
   - `AmazonEC2FullAccess`
   - `CloudWatchLogsFullAccess`
4. Name the role: `lambda-ec2-control-role`
5. Click **Create role**

✅ You now have an instance and IAM role ready.

---

## ⚙️ Part 2 — Lambda to Stop EC2 Instance

### 🧱 Step 1: Create Stop Lambda Function
1. Go to **Lambda → Create function**
2. Options:
   - Name: `stop-ec2-instance`
   - Runtime: Python 3.12
   - Role: `lambda-ec2-control-role`
3. Code:
   ```python
   import boto3

   def lambda_handler(event, context):
       ec2 = boto3.client('ec2')
       instance_ids = ['i-0123456789abcdef0']  # Replace with your Instance ID
       response = ec2.stop_instances(InstanceIds=instance_ids)
       print(f"Stopping instances: {instance_ids}")
       return response
   ```
4. Deploy → Test → confirm output.

✅ Lambda now stops the EC2 instance manually.

### ⏰ Step 2: Schedule Stop Function (Cron)
1. Inside `stop-ec2-instance` → **Configuration → Triggers → Add trigger**
2. Choose **EventBridge (CloudWatch Events)**
3. Create new rule:
   - Name: `stop-ec2-schedule`
   - Schedule expression:
     ```
     cron(0 20 * * ? *)
     ```
     (Runs every day at 8 PM UTC)
4. Save.

✅ The instance will now stop automatically at 8 PM UTC.

---

## ⚡ Part 3 — Lambda to Start EC2 Instance

### 🧱 Step 3: Create Start Lambda Function
1. Go to **Lambda → Create function**
2. Options:
   - Name: `start-ec2-instance`
   - Runtime: Python 3.12
   - Role: `lambda-ec2-control-role`
3. Code:
   ```python
   import boto3

   def lambda_handler(event, context):
       ec2 = boto3.client('ec2')
       instance_ids = ['i-0123456789abcdef0']  # Replace with your Instance ID
       response = ec2.start_instances(InstanceIds=instance_ids)
       print(f"Starting instances: {instance_ids}")
       return response
   ```
4. Deploy → Test → verify success.

✅ Lambda now starts the EC2 instance manually.

### 🗓️ Step 4: Schedule Start Function (Cron)
1. Inside `start-ec2-instance` → **Configuration → Triggers → Add trigger**
2. Choose **EventBridge (CloudWatch Events)**
3. Create new rule:
   - Name: `start-ec2-schedule`
   - Schedule expression:
     ```
     cron(0 8 * * ? *)
     ```
     (Runs every day at 8 AM UTC)
4. Save.

✅ The instance will now start automatically at 8 AM UTC.

---

## ✅ Final Setup Summary

| Lambda Function | Purpose | Schedule | Action |
|------------------|----------|-----------|--------|
| `start-ec2-instance` | Start instance daily | 8 AM UTC | `start_instances()` |
| `stop-ec2-instance`  | Stop instance daily  | 8 PM UTC | `stop_instances()`  |

---

## 🧹 Cleanup (Optional)
To avoid extra usage:
- Delete both Lambda functions
- Delete EventBridge rules (`start-ec2-schedule`, `stop-ec2-schedule`)
- (Optional) Delete the IAM role `lambda-ec2-control-role`

---

✅ **You have successfully automated EC2 instance start and stop with AWS Lambda + Cron (EventBridge).**