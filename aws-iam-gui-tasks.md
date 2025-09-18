# AWS IAM GUI-Based Tasks

This guide contains step-by-step **IAM practice tasks** covering **users, groups, policies, permissions boundaries, credential reports, and service-linked roles**.

---

## ✅ Task 1: Create a New IAM User
1. Go to **IAM → Users → Add User**.
2. Username: **DevUser**.
3. Select **AWS Management Console access**.
4. Set custom password.
5. Attach **AmazonEC2ReadOnlyAccess**.
6. Save and log in as DevUser.

---

## ✅ Task 2: Create an IAM Group
1. Go to **Groups → Create New Group**.
2. Name: **Developers**.
3. Attach policy: **AmazonS3FullAccess**.
4. Add **DevUser** to this group.

---

## ✅ Task 3: Attach Additional Policy to User
1. Go to **Users → DevUser → Permissions**.
2. Attach policy: **AmazonEC2FullAccess**.
3. DevUser now has both **S3 Full Access** and **EC2 Full Access**.

---

## ✅ Task 4: Enable MFA for User
1. Go to **Users → DevUser → Security Credentials**.
2. Enable **Multi-Factor Authentication (MFA)**.
3. Use **Virtual MFA app** (Google Authenticator/Authy).
4. Test login with MFA.

---

## ✅ Task 5: Create IAM Role for EC2
1. Go to **Roles → Create Role**.
2. Select **EC2** as trusted entity.
3. Attach **AmazonS3ReadOnlyAccess**.
4. Launch an EC2 instance and attach this role.
5. SSH into instance and test with:
   ```bash
   aws s3 ls
   ```

## ✅ Task 6: Create a Customer Managed Policy
1. Go to **Policies → Create Policy**.
2. Service: **EC2**.
3. Allow only:
   - `DescribeInstances`
   - `StartInstances`
   - `StopInstances`
4. Save as **EC2LimitedAccess**.
5. Attach to **DevUser**.
6. Test login → confirm limited access.

---

## ✅ Task 7: Set Permissions Boundaries
1. Go to **Users → DevUser → Permissions Boundary**.
2. Create a policy that only allows S3 access.
3. Attach it as boundary.
4. Even if extra policies are added, user cannot exceed boundary.

---

## ✅ Task 8: Create IAM Access Keys
1. Go to **Users → DevUser → Security Credentials**.
2. Create **Access Key**.
3. Configure AWS CLI locally:
   ```bash
   aws configure
```

---

## ✅ Task 9: Use IAM Policy Simulator
1. Go to **Policy Simulator** in IAM.
2. Select **DevUser**.
3. Test actions like `s3:PutObject` and `ec2:TerminateInstances`.
4. Verify allowed/denied actions.

---

## ✅ Task 10: Enable IAM Credential Report & Access Analyzer
1. Go to **Credential Report → Download Report**.
   - Check which users have active keys, MFA, etc.
2. Go to **Access Analyzer → Create Analyzer**.
   - Review external resource sharing (e.g., S3 buckets).

---

## ✅ Task 11: Create Multiple Groups with Different Permissions
1. Create groups:
   - **Admins** → `AdministratorAccess`
   - **Developers** → `AmazonEC2FullAccess`, `AmazonS3FullAccess`
   - **Auditors** → `ReadOnlyAccess`
   - **BillingTeam** → `AWSBillingConductorFullAccess`

---

## ✅ Task 12: Create Multiple Users and Assign Them to Groups
1. Create the following users:
   - **Alice** → Admins
   - **Bob** → Developers
   - **Charlie** → Auditors
   - **Diana** → BillingTeam
2. Assign **console + programmatic access** for Admins/Developers.
3. Assign **console-only access** for Auditors/BillingTeam.

---

## ✅ Task 13: Test Group Policies
1. Log in as **Alice** → verify full access.
2. Log in as **Bob** → try launching EC2 and uploading to S3.
3. Log in as **Charlie** → confirm read-only restrictions.
4. Log in as **Diana** → verify billing console access but no EC2 creation rights.

---

## ✅ Task 14: Nested Access via Additional Policies
1. Add **AmazonRDSReadOnlyAccess** directly to **Charlie**.
2. Test Charlie’s ability to view RDS databases even though in Auditors group.

---

## ✅ Task 15: Apply Password Policies
1. Go to **IAM → Account Settings → Password Policy**.
2. Enforce:
   - Minimum length 12
   - Require numbers, symbols, uppercase, lowercase
   - Password expiration: 90 days
   - Prevent password reuse
3. Test with a new user creation.

---

## ✅ Task 16: Create Service-Linked Role
1. Go to **IAM → Roles → Create Role**.
2. Select a service like **Elastic Load Balancing**.
3. Create role and observe how AWS manages it automatically.

---
