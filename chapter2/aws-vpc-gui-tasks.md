# AWS VPC GUI-Based Practice Tasks

This guide provides **step-by-step tasks** for practicing Amazon VPC using the AWS Management Console.

---

## ✅ Task 1: Create Your First VPC
1. Go to **VPC Dashboard → Your VPCs → Create VPC**.
2. Choose **VPC only** option.
3. Name it **MyFirstVPC**.
4. Set **IPv4 CIDR block**: `10.0.0.0/16`.
5. Leave defaults and create.

---

## ✅ Task 2: Create Subnets
1. Go to **Subnets → Create Subnet**.
2. Select **MyFirstVPC**.
3. Create **two subnets**:
   - Public Subnet: `10.0.1.0/24`
   - Private Subnet: `10.0.2.0/24`
4. Assign them to different **Availability Zones** for high availability.

---

## ✅ Task 3: Configure Internet Gateway (IGW)
1. Go to **Internet Gateways → Create Internet Gateway**.
2. Name it **MyIGW**.
3. Attach it to **MyFirstVPC**.

---

## ✅ Task 4: Configure Route Tables
1. Go to **Route Tables → Create Route Table**.
2. Name it **PublicRT** and associate it with **MyFirstVPC**.
3. Add route:
   - Destination: `0.0.0.0/0`
   - Target: **MyIGW**
4. Associate **Public Subnet** with **PublicRT**.

---

## ✅ Task 5: Launch EC2 in Public Subnet
1. Go to **EC2 → Launch Instance**.
2. Choose **MyFirstVPC → Public Subnet**.
3. Enable **Auto-assign Public IP**.
4. Launch instance and test SSH/web access.

---

## ✅ Task 6: Create a NAT Gateway
1. Go to **NAT Gateways → Create NAT Gateway**.
2. Place it in **Public Subnet**.
3. Allocate an **Elastic IP** for it.
4. Create a new **Route Table** → Name: **PrivateRT**.
5. Add route:
   - Destination: `0.0.0.0/0`
   - Target: **NAT Gateway**
6. Associate **Private Subnet** with **PrivateRT**.

---

## ✅ Task 7: Launch EC2 in Private Subnet
1. Launch EC2 into **Private Subnet**.
2. Confirm it **has no public IP**.
3. From private EC2, test:
   ```bash
   ping google.com
   ```
   → Works through **NAT Gateway**.

---

## ✅ Task 8: Create Security Groups
1. Create **WebSG**:
   - Allow inbound HTTP (80) from anywhere.
   - Allow SSH (22) from your IP.
2. Create **DBSG**:
   - Allow inbound MySQL (3306) only from **WebSG**.
3. Assign WebSG to **Public EC2** and DBSG to **Private EC2**.

---

## ✅ Task 9: Create a VPC Peering Connection
1. Create another VPC: **MySecondVPC** with CIDR `10.1.0.0/16`.
2. Go to **Peering Connections → Create Peering Connection**.
3. Request: **MyFirstVPC → MySecondVPC**.
4. Accept the peering connection.
5. Update route tables in both VPCs for mutual communication.

---

## ✅ Task 10: Enable Flow Logs
1. Go to **VPC Dashboard → Your VPCs → Select MyFirstVPC**.
2. Choose **Flow Logs → Create Flow Log**.
3. Destination: **CloudWatch Logs**.
4. Generate traffic and confirm logs in CloudWatch.

---
