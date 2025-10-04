# ☁️ AWS RDS & DynamoDB Practice Tasks

---

## 📘 AWS RDS Practice Tasks

### 🎯 Goal
Learn to create, connect, and manage an Amazon RDS database using the AWS Management Console and CLI.

---

### 🪜 Task 1: Create RDS Instance
**Steps:**
1. Open **AWS Console → RDS**
2. Click **Create database**
3. Choose:
   - Engine: **MySQL / PostgreSQL**
   - Deployment: **Standard create**
   - Template: **Free tier**
4. Set:
   - DB instance identifier: `mydb-instance`
   - Master username: `admin`
   - Password: `YourPassword123`
5. Storage type: **gp3 (20 GB)**
6. Enable **Public access** (for testing)
7. Create the DB

🧠 *Verify:* Wait until status = `Available`.

---

### 🪜 Task 2: Connect to Database
**From Local Machine or EC2:**
```bash
mysql -h <RDS-endpoint> -P 3306 -u admin -p
```
🧩 *Note:* Ensure inbound rule for **port 3306** is open in the RDS security group.

---

### 🪜 Task 3: Create and Manage Database Tables
```sql
CREATE DATABASE company;
USE company;

CREATE TABLE employees (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  department VARCHAR(50),
  salary DECIMAL(10,2)
);

INSERT INTO employees (name, department, salary)
VALUES ('John Doe', 'IT', 65000.00);

SELECT * FROM employees;
```

---

### 🪜 Task 4: Backup and Restore
1. Take a **manual snapshot** from AWS Console.
2. Delete the DB instance.
3. Restore DB from snapshot → rename as `mydb-restore`.
4. Verify restored DB connectivity.

---

### 🪜 Task 5: Monitor and Scale
- View **CloudWatch metrics** for CPU, IOPS, and connections.
- Modify instance type from `db.t3.micro` → `db.t3.small` (simulate scaling).

---
---

## 📗 AWS DynamoDB Practice Tasks

### 🎯 Goal
Understand key-value and NoSQL table creation, CRUD operations, and indexing.

---

### 🪜 Task 1: Create DynamoDB Table
**Steps:**
1. Open **AWS Console → DynamoDB → Tables → Create table**
2. Table name: `EmployeeData`
3. Partition key: `EmployeeID (String)`
4. Keep default settings and create.

---

### 🪜 Task 2: Insert Items
**CLI Example:**
```bash
aws dynamodb put-item --table-name EmployeeData --item '{"EmployeeID": {"S": "E102"}, "Name": {"S": "Bob"}, "Role": {"S": "Manager"}, "Salary": {"N": "90000"}}'
```

---

### 🪜 Task 3: Retrieve Items
```bash
aws dynamodb get-item --table-name EmployeeData --key '{"EmployeeID": {"S": "E101"}}'
```

---

### 🪜 Task 4: Scan and Query
```bash
# Scan all items
aws dynamodb scan --table-name EmployeeData

# Query by key
aws dynamodb query --table-name EmployeeData --key-condition-expression "EmployeeID = :id" --expression-attribute-values '{":id":{"S":"E101"}}'
```

---

### 🪜 Task 5: Add Global Secondary Index
1. Add new index:
   - Index name: `RoleIndex`
   - Partition key: `Role (String)`
2. Query the table by Role via Console or CLI.

---

### 🪜 Task 6: Delete Table
```bash
aws dynamodb delete-table --table-name EmployeeData
```

🧠 *Verify:* Use `aws dynamodb list-tables` to confirm deletion.

---

### 🏁 Bonus Challenge
Integrate both services:
- Store relational data in **RDS**
- Store metadata (e.g., access logs or user preferences) in **DynamoDB**
- Use **AWS Lambda** to sync new RDS entries to DynamoDB.
