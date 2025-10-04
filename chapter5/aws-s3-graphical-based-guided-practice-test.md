# 🧠 AWS S3 Practice Tasks (Graphical / Console Only)

## 🎯 Goal:
Learn S3 hands-on using the **AWS Management Console** (no command line required).

---

## 🧩 Section 1: Basic Operations

### 🪣 Task 1: Create an S3 Bucket
1. Log in to the **AWS Management Console** → open **S3**.
2. Click **Create bucket**.
3. Enter a unique bucket name → e.g., `my-first-s3-bucket-manvir`.
4. Choose region: **Asia Pacific (Mumbai) ap-south-1**.
5. Leave “Block all public access” **checked**.
6. Enable **Bucket Versioning**.
7. Click **Create bucket**.

---

### 📂 Task 2: Upload and Manage Objects
1. Open your new bucket.
2. Click **Upload → Add files** → select `hello.txt`.
3. Click **Upload**.
4. Once uploaded, click the object name → **Object URL** (try opening it – should show Access Denied).
5. Select the object → **Actions → Make public using ACL**.
6. Try the URL again — it should now open successfully.

---

### 🗂️ Task 3: Create Folders and Upload Files
1. Inside the bucket → click **Create folder** → name it `images/`.
2. Upload a few image files into this folder.
3. Confirm they appear under the correct path.

---

## 🧱 Section 2: Permissions and Access Control

### 🔒 Task 4: Add a Bucket Policy
1. Open your bucket → **Permissions** tab.
2. Scroll to **Bucket Policy** → click **Edit**.
3. Add a JSON policy that allows public read (paste from AWS Policy Generator).
4. Save and test object access again via URL.

---

### 👥 Task 5: IAM Role / User Access
1. Go to **IAM** → **Users** → **Add user**.
2. Give access type **Programmatic and Console access**.
3. Attach permissions → choose **AmazonS3FullAccess** (for testing only).
4. Log in as the new user → confirm S3 access works.

---

## 💾 Section 3: Storage and Lifecycle

### 🧊 Task 6: Change Storage Class
1. Upload 3 files → then select them.
2. Choose **Actions → Change storage class**.
3. Change one to:
   - **Standard**
   - **Standard-IA**
   - **Glacier Flexible Retrieval**

---

### ⏳ Task 7: Lifecycle Rule
1. Go to **Management** tab of your bucket.
2. Click **Create lifecycle rule**.
3. Example rule:
   - Move to **Standard-IA** after 30 days.
   - Move to **Glacier** after 60 days.
   - Permanently delete after 120 days.
4. Save the rule.

---

## 🔐 Section 4: Security & Logging

### 🪶 Task 8: Enable Default Encryption
1. Go to **Properties** tab.
2. Scroll to **Default encryption**.
3. Choose **Enable** → **SSE-S3 (AES-256)**.
4. Save changes.

---

### 📜 Task 9: Enable Server Access Logging
1. Create another bucket: `my-log-bucket-manvir`.
2. Open the first bucket → **Properties → Server access logging**.
3. Enable logging → choose `my-log-bucket-manvir` as the target.
4. Save and confirm setup.

---

## 🌐 Section 5: Static Website Hosting

### 🕸️ Task 10: Host a Static Website
1. Open your bucket → **Properties** tab.
2. Scroll to **Static website hosting** → click **Edit**.
3. Enable and enter:
   - Index document: `index.html`
   - Error document: `error.html`
4. Upload both files under your bucket.
5. Save and visit the **Endpoint URL** shown.

---

## 🚀 Section 6: Advanced Practice

### 🔁 Task 11: Cross-Region Replication
1. Enable **Versioning** in both buckets.
2. Go to **Management → Replication → Create rule**.
3. Choose destination bucket in another region.
4. Grant replication permissions (automatically).
5. Test by uploading a file to see if it replicates.

---

### 🧩 Task 12: Versioning and Object Restore
1. Modify and upload multiple versions of `hello.txt`.
2. Click **Show versions** in your bucket.
3. Restore an older version by downloading and re-uploading it.

---

### ⏱️ Task 13: Presigned URL (via Console)
1. Select an object → click **Actions → Share with a presigned URL**.
2. Set expiration time (e.g., 1 hour).
3. Copy the link and test temporary access.

---

### 🧹 Task 14: Cleanup
1. Empty the bucket (select all → Delete).
2. Go back → **Delete bucket**.
3. Confirm deletion of both buckets.

---
