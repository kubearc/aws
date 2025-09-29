# AWS EFS Practice Tasks

This file contains hands-on practice tasks for learning Amazon Elastic File System (EFS) in AWS.

---

## 🔹 Beginner Level

1. **Create an EFS File System**
   - Go to AWS Console → EFS → Create File System.
   - Use **General Purpose** performance mode.
   - Enable **Automatic Backups**.

2. **Create Mount Targets**
   - Ensure you create mount targets in at least 2 Availability Zones (AZs).
   - Attach to a VPC security group that allows NFS traffic (port 2049).

3. **Launch an EC2 Instance**
   - Use Amazon Linux 2 or Ubuntu.
   - Attach the same VPC + security group as EFS.

4. **Mount EFS to EC2**
   - Install the EFS mount helper:
     ```bash
     sudo yum install -y amazon-efs-utils   # (Amazon Linux)
     ```
   - Mount the EFS:
     ```bash
     sudo mkdir /mnt/efs
     sudo mount -t efs fs-XXXXXXXX:/ /mnt/efs
     ```

---

## 🔹 Intermediate Level

5. **Test File Sharing**
   - Launch **two EC2 instances** in different AZs.
   - Mount the same EFS file system on both.
   - Create a file from instance A:
     ```bash
     echo "Hello from Instance A" | sudo tee /mnt/efs/testfile.txt
     ```
   - Check if instance B can see the same file.

6. **Enable Encryption**
   - Create another EFS with **encryption at rest enabled**.
   - Mount and confirm encryption is enabled using console/CLI.

7. **Use Access Points**
   - Create an **EFS Access Point** with a specific POSIX user & directory.
   - Mount via:
     ```bash
     sudo mount -t efs -o tls,accesspoint=fsap-xxxxxx fs-xxxxxx:/ /mnt/efs-ap
     ```
   - Verify that permissions are enforced.

---

## 🔹 Advanced Level

8. **Performance Modes**
   - Create two EFS file systems:
     - One with **General Purpose** mode.
     - One with **Max I/O** mode.
   - Run I/O performance tests (e.g., using `dd` or `fio`) to compare.

9. **Throughput Modes**
   - Change EFS to **Provisioned Throughput** mode.
   - Measure file copy speeds before & after.

10. **Lifecycle Management**
    - Enable lifecycle policy (e.g., move files to Infrequent Access after 7 days).
    - Upload a test file and wait for transition.

11. **Mount via ECS / EKS**
    - Configure EFS as persistent storage in a containerized environment.
    - (e.g., use an EFS CSI driver in EKS).

12. **Automated Mounting**
    - Add an entry to `/etc/fstab`:
      ```bash
      fs-XXXXXXXX:/ /mnt/efs efs defaults,_netdev 0 0
      ```
    - Reboot EC2 and confirm it mounts automatically.

---

