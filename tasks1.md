# 📝 Practice Test: Linux + Podman + AWS (RHEL 9)

## Section 1: AWS Setup
1. Create a **new VPC** with:  
   - CIDR block: `10.0.0.0/16`  
   - One **public subnet** (`10.0.1.0/24`)  
   - One **private subnet** (`10.0.2.0/24`)  
   - An Internet Gateway attached to the VPC and associated with the public subnet.  

2. Create a **Security Group** that:  
   - Allows inbound SSH (22) from your IP  
   - Allows inbound HTTP (80) from anywhere  

3. Launch a **RHEL 9 EC2 instance** in the public subnet with:  
   - Instance type: `t2.micro`  
   - Security group created above  
   - Key pair for SSH access  

---

## Section 2: Linux Basics (on EC2)
1. Connect to the EC2 instance via SSH.  
2. Create a new user `devops` and allow it to run `sudo` commands.  
3. Create a directory `/appdata` and set correct ownership for `devops`.  
4. Create a cron job for user `devops` that runs `uptime` every 5 minutes and appends the output to `/appdata/sysusage.log`.  
5. Check disk usage with `df -h` and redirect the output to `/appdata/disk_report.txt`.  

---

## Section 3: Podman Basics (on EC2)
1. Install Podman on the EC2 instance. Verify installation with `podman --version`.  
2. Run a container with `nginx` named `web1` and expose it on port 80.  
3. Test the container by accessing `http://<EC2-Public-IP>` in a browser.  
4. Create a Pod named `mypod` and add a `redis` container inside it.  
5. Build a simple Podman image:  
   - Create a Dockerfile with `ubi9` base image  
   - Add a text file `/hello.txt` inside the image  
   - Run the image and confirm the file exists  

---

## Section 4: AWS Networking & Storage
1. Launch another EC2 instance (`RHEL 9`, same VPC) in the **private subnet**.  
   - Ensure it has no direct internet access.  
   - Allow only SSH from the public EC2 instance (bastion host).  
   - Connect from public EC2 → private EC2 via SSH.  

2. Create and attach a new **EBS volume (2GB)** to the public EC2 instance.  
   - Format it as `xfs`  
   - Mount it to `/mnt/ebs`  
   - Ensure it persists after reboot  

---

## Section 5: Load Balancer & Auto Scaling
1. Launch **two additional EC2 instances** (RHEL 9) in the public subnet.  
   - Install Podman on both.  
   - Run an `nginx` container on each instance, but configure them to show different index pages (`Server1`, `Server2`, `Server3`).  

2. Create an **Application Load Balancer (ALB)**:  
   - Target group should include all 3 EC2 instances.  
   - Listener: Port 80 (HTTP).  
   - Test using the ALB DNS name — it should round-robin across the 3 servers.  

3. Create a **Launch Template** that:  
   - Uses RHEL 9 AMI.  
   - Installs Podman at boot (via user data).  
   - Runs an `nginx` container automatically.  

4. Create an **Auto Scaling Group (ASG)**:  
   - Attach it to the public subnet.  
   - Desired capacity: 2  
   - Min size: 1  
   - Max size: 3  
   - Attach it to the same ALB created earlier.  

5. Test scaling:  
   - Increase desired capacity to 3 and verify a new instance is launched and registered with the ALB.  
   - Terminate one instance manually and check if ASG replaces it automatically.  

---

## Section 6: Scenario-Based Tasks

1. **Restricted Access**:  
   - Modify the Security Group of your EC2 instance to allow inbound HTTP (80) only from your IP.  
   - Verify that the site is not accessible from other IPs.  

2. **Staging Environment**:  
   - Create another VPC (same CIDR ranges).  
   - Launch one RHEL 9 EC2 in that staging VPC.  
   - Install Podman and run an `nginx` container.  
   - Configure **VPC peering** so that staging and production VPCs can communicate.  

---

