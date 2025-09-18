# AWS EC2 GUI-Based Practice Tasks

This guide provides step-by-step **hands-on practice tasks** for Amazon EC2 using the AWS Management Console.

---

## ✅ Task 1: Launch Your First EC2 Instance
1. Go to **EC2 Dashboard → Instances → Launch Instances**.
2. Select an **Amazon Linux  AMI** (free tier).
3. Choose **t2.micro** instance type (free tier).
4. Create a **new key pair** → download `.pem` file.
5. Create a **new security group**:
   - Allow SSH (port 22) → Your IP.
   - Allow HTTP (port 80) → Anywhere.
6. Launch the instance and note the **public IP**.

---

## ✅ Task 2: Connect to Your EC2 Instance
1. Select your instance → click **Connect**.
2. Choose **EC2 Instance Connect** (browser-based SSH).
3. Run:
   ```bash
   sudo yum update -y
   ```

---

## ✅ Task 3: Deploy a Web Server
1. On your EC2 instance:
   ```bash
   sudo yum install httpd -y
   sudo systemctl start httpd
   sudo systemctl enable httpd
   echo "Hello from EC2 Web Server" | sudo tee /var/www/html/index.html
   ```
2. Open a browser → visit your **EC2 Public IP** → you should see your message.

---

## ✅ Task 4: Create and Attach an EBS Volume
1. Go to **Elastic Block Store → Volumes → Create Volume**.
2. Choose same **Availability Zone** as your EC2.
3. Attach the volume to your EC2 instance.
4. On EC2:
   ```bash
   lsblk
   sudo mkfs -t ext4 /dev/xvdf
   sudo mkdir /data
   sudo mount /dev/xvdf /data
   ```
   **Note** Replace **xvdf** With your drive name 
5. Verify with `df -h`.

---

## ✅ Task 5: Create an AMI from Your Instance
1. Right-click EC2 → **Create Image (AMI)**.
2. Name it **MyFirstAMI**.
3. Launch a new instance from this AMI → confirm your web server works.

---

## ✅ Task 6: Create Security Groups & Test Rules
1. Create a new **Security Group** allowing only ICMP (ping).
2. Assign it to your EC2.
3. From another system, try to ping → should work.
4. Try SSH → should fail.

---

## ✅ Task 7: Set Up Elastic IP
1. Go to **Elastic IPs → Allocate Elastic IP**.
2. Associate it with your EC2 instance.
3. Access your site with the Elastic IP instead of the public IP.

---

## ✅ Task 8: Create Auto Recovery Alarm
1. Go to **CloudWatch → Alarms → Create Alarm**.
2. Select **EC2 → Status Check Failed (Instance)**.
3. Set an alarm action → Recover this instance.
4. Simulate by stopping instance → start again → alarm triggers.

---

## ✅ Task 9: Create a Load Balancer
1. Go to **EC2 → Load Balancers → Create Load Balancer**.
2. Choose **Application Load Balancer (ALB)**.
3. Add at least **2 EC2 instances** behind it.
4. Test ALB DNS → should load balance requests.

---

## ✅ Task 10: ---

## ✅ Step 1: Create Auto Scaling Group
1. Go to **EC2 → Auto Scaling Groups → Create Auto Scaling Group**.
2. Attach a **Launch Template** (or Launch Configuration).
3. Select the required **VPC and Subnets**.
4. Continue with default or required settings.

---

## ✅ Step 2: Set Capacity
- Minimum: `0` (instances can scale down to zero → stopped).
- Maximum: `1` (only one instance allowed).
- Desired: `0` (stopped by default).

---

## ✅ Step 3: Add Scheduled Actions
1. Go to your **Auto Scaling Group → Scheduled Actions → Create Scheduled Action**.
2. Create two actions:
   - **Start Action**
     - Desired = 1
     - Min = 1
     - Max = 1
     - Schedule: e.g., `08:00 AM` daily
   - **Stop Action**
     - Desired = 0
     - Min = 0
     - Max = 0
     - Schedule: e.g., `08:00 PM` daily

---

## ✅ Step 4: Confirm Behavior
- At **start time**, Auto Scaling launches one EC2 instance.
- At **stop time**, Auto Scaling terminates the EC2 instance.

---

