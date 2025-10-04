# 🧠 AWS S3 Practice Tasks (CLI-Based)

## 🎯 Goal:
Learn and practice S3 operations using the **AWS Command Line Interface (CLI)** for full hands-on control.

---

## 🧩 Section 1: Basic Operations

### 🪣 Task 1: Create an S3 Bucket
```bash
aws s3api create-bucket --bucket my-first-s3-bucket-manvir --region ap-south-1 --create-bucket-configuration LocationConstraint=ap-south-1
aws s3api put-bucket-versioning --bucket my-first-s3-bucket-manvir --versioning-configuration Status=Enabled
```

---

### 📂 Task 2: Upload and Manage Objects
```bash
echo "Hello S3!" > hello.txt
aws s3 cp hello.txt s3://my-first-s3-bucket-manvir/
aws s3api put-object-acl --bucket my-first-s3-bucket-manvir --key hello.txt --acl public-read
```

---

### 🗂️ Task 3: Folder and Multiple Files
```bash
mkdir images
cp hello.txt images/
aws s3 sync ./images s3://my-first-s3-bucket-manvir/images/
```

---

## 🧱 Section 2: Permissions and Access Control

### 🔒 Task 4: Bucket Policy
```bash
cat > policy.json <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": ["s3:GetObject"],
      "Resource": ["arn:aws:s3:::my-first-s3-bucket-manvir/*"]
    }
  ]
}
EOF

aws s3api put-bucket-policy --bucket my-first-s3-bucket-manvir --policy file://policy.json
```

---

### 👥 Task 5: IAM Role for S3
1. Create an IAM role with permissions for S3:
   ```bash
   aws iam create-role --role-name S3AccessRole --assume-role-policy-document file://trust-policy.json
   ```
2. Attach S3 access policy:
   ```bash
   aws iam attach-role-policy --role-name S3AccessRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
   ```

---

## 💾 Section 3: Storage Classes and Lifecycle

### 🧊 Task 6: Object Storage Class
```bash
aws s3 cp file1.txt s3://my-first-s3-bucket-manvir/ --storage-class STANDARD
aws s3 cp file2.txt s3://my-first-s3-bucket-manvir/ --storage-class STANDARD_IA
aws s3 cp file3.txt s3://my-first-s3-bucket-manvir/ --storage-class GLACIER
```

---

### ⏳ Task 7: Lifecycle Policy
```bash
cat > lifecycle.json <<EOF
{
  "Rules": [
    {
      "ID": "MoveToIAAfter30Days",
      "Filter": {"Prefix": ""},
      "Status": "Enabled",
      "Transitions": [
        {"Days": 30, "StorageClass": "STANDARD_IA"},
        {"Days": 60, "StorageClass": "GLACIER"}
      ],
      "Expiration": {"Days": 120}
    }
  ]
}
EOF

aws s3api put-bucket-lifecycle-configuration --bucket my-first-s3-bucket-manvir --lifecycle-configuration file://lifecycle.json
```

---

## 🔐 Section 4: Encryption & Logging

### 🪶 Task 8: Bucket Encryption
```bash
aws s3api put-bucket-encryption --bucket my-first-s3-bucket-manvir --server-side-encryption-configuration '{
  "Rules": [{"ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "AES256"}}]
}'
```

---

### 📜 Task 9: Bucket Logging
```bash
aws s3api create-bucket --bucket my-log-bucket-manvir --region ap-south-1 --create-bucket-configuration LocationConstraint=ap-south-1

aws s3api put-bucket-logging --bucket my-first-s3-bucket-manvir --bucket-logging-status '{
  "LoggingEnabled": {
    "TargetBucket": "my-log-bucket-manvir",
    "TargetPrefix": "logs/"
  }
}'
```

---

## 🌐 Section 5: Static Website Hosting

### 🕸️ Task 10: Static Website Hosting
```bash
aws s3 cp index.html s3://my-first-s3-bucket-manvir/
aws s3 cp error.html s3://my-first-s3-bucket-manvir/

aws s3 website s3://my-first-s3-bucket-manvir/ --index-document index.html --error-document error.html
```

---

## 🚀 Section 6: Advanced Features

### 🔁 Task 11: Cross-Region Replication
1. Create a replication bucket:
   ```bash
   aws s3api create-bucket --bucket my-s3-replica-manvir --region us-east-1
   ```
2. Enable replication (after enabling versioning in both):
   ```bash
   aws s3api put-bucket-replication --bucket my-first-s3-bucket-manvir --replication-configuration file://replication.json
   ```

---

### 🧩 Task 12: Versioning and Recovery
```bash
aws s3api list-object-versions --bucket my-first-s3-bucket-manvir
aws s3api delete-object --bucket my-first-s3-bucket-manvir --key hello.txt --version-id <version-id>
```

---

### ⏱️ Task 13: Presigned URL
```bash
aws s3 presign s3://my-first-s3-bucket-manvir/hello.txt --expires-in 3600
```

---

### 🧹 Task 14: Cleanup
```bash
aws s3 rm s3://my-first-s3-bucket-manvir --recursive
aws s3api delete-bucket --bucket my-first-s3-bucket-manvir
```

---

✅ **End of AWS S3 CLI Practice Tasks**
