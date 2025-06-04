# REPPORT ON IAM POLICIES, SECURE STORAGE & DATA ENCRYTPTION 
## Iam User Configuration 

## 👤 1. Root User
Who it is:
The owner of the AWS account. This user is created when you first sign up for AWS using an email and password.

### Access Level:
✅ Full, unrestricted access to all AWS services and account-level settings

### Abilities:

Can access and modify everything in AWS

Can manage billing, subscriptions, and close the AWS account

Can create/delete IAM users and assign any permissions

Can enable/disable Multi-Factor Authentication (MFA)

Can delete S3 buckets, EC2 instances, RDS, etc.

### Restrictions:

⚠️ Cannot be restricted by IAM policies

Not recommended for daily use

Best practice: Use only for critical operations and setup MFA
![Screenshot 2025-06-04 134434](https://github.com/user-attachments/assets/12100d68-b556-4faa-98c8-cb376bbdf6df)
Root User can create users and iam dashboard for Root user

# 👤 2. IAM User: iamuser
## Who it is:
A user created for basic access, such as interns, QA testers, or third-party apps that only need read-only access to files in S3.

Policy Attached:
Custom policy named S3-readonly

Access Level:
🔒 Read-only access to a specific S3 bucket

### Abilities:

✅ List S3 buckets

✅ View/download files from the allowed bucket

### Restrictions:

❌ Cannot upload, delete, or edit files

❌ Cannot access any other AWS service (like EC2, RDS, IAM, etc.)
![Screenshot 2025-06-04 134137](https://github.com/user-attachments/assets/bae1822d-e878-4578-851c-88cc11a03045)
resticting access for user
👨‍💻 3. IAM User: developer-user
Who it is:
A user meant for developers who need full control over S3 and read-only access to EC2 — typically used in application testing or development environments.

### Policies Attached:

AmazonS3FullAccess

AmazonEC2ReadOnlyAccess

### Access Level:
✅ Full access to S3
🔍 Read-only access to EC2

#### Abilities:

✅ Upload, modify, delete files in any S3 bucket

✅ View EC2 instance details (public/private IPs, type, state, etc.)

Restrictions:

❌ Cannot stop/start/terminate EC2 instances

❌ Cannot manage IAM, billing, or root settings
![Screenshot 2025-06-04 134351](https://github.com/user-attachments/assets/03ce7cdf-9820-4afd-9ffa-126066ecbc31)
showing the access of developer 
## 🛠️ 4. IAM User: admin-user
Who it is:
A user with AdministratorAccess — similar to the root user, but still restricted from some account-level operations.

Policy Attached:
AWS-managed AdministratorAccess policy

Access Level:
✅ Full access to most AWS services

### Abilities:

✅ Create/edit/delete S3, EC2, Lambda, RDS, etc.

✅ Manage IAM users, groups, and roles

✅ Configure VPC, security groups, Route 53, etc.

### Restrictions:

❌ Cannot change root account settings or delete the AWS account

❌ May not have access to billing dashboard unless explicitly allowed

❌ Cannot reset root password or MFA
![Screenshot 2025-06-04 142946](https://github.com/user-attachments/assets/01cc414b-f494-4bf0-a776-a3df93247d12)
Admin access 
## Compare root user and admin-user

| Feature / Action                                          | **Root User** ✅              | **Admin IAM User** ✅             |
| --------------------------------------------------------- | ----------------------------- |   -------------------------------- |
| Created during AWS account sign-up                        | ✅ Yes                        | ❌ No (created by root/admin)     |
| Full access to all AWS services                           | ✅ Yes                        | ✅ Yes                            |
| Access to Billing and Cost Management                     | ✅ Yes                        | ❌ No (unless explicitly allowed) |
| Can create, manage, or delete IAM users/roles             | ✅ Yes                        | ✅ Yes                            |
| Can enable/disable root Multi-Factor Authentication (MFA) | ✅ Yes                        | ❌ No                             |
| Can delete/close the AWS account                          | ✅ Yes                        | ❌ No                             |
| Can reset root password or credentials                    | ✅ Yes                        | ❌ No                             |
| Can be assigned IAM policies                              | ❌ No                         | ✅ Yes                            |
| Can be restricted by IAM permissions                      | ❌ No                         | ✅ Yes                            |
| Intended for daily use                                    | ❌ Not recommended            | ✅ Yes                            |
| Best Practice                                             | Use only for sensitive tasks.  | Use for regular admin work        |
![Screenshot 2025-06-04 142508](https://github.com/user-attachments/assets/7f2f1351-56d1-44a2-8585-b3288a53d38a)
All users - 1 admin, 3 developer , 1 view user

# 🔐 Secure Storage Implementation
To secure data stored in the cloud, we implemented the following best practices using AWS S3:

## ✅ 1. Private S3 Buckets
All S3 buckets are configured to block public access

Ensures files can't be accessed by unauthorized users over the internet

## ✅ 2. Bucket Policies and IAM Roles
Access to S3 buckets is controlled through IAM policies

Read-only access for iamuser

Full access for developer-user

Fine-grained permissions prevent unauthorized modifications

## ✅ 3. Server-Side Encryption (SSE)
SSE-S3 or SSE-KMS enabled for bucket:

Encrypts files automatically at rest

Protects sensitive data from exposure

Verified under Bucket Properties → Default Encryption

## ✅ 4. Versioning (Optional)
Enables recovery from accidental deletes or overwrites

Recommended for sensitive data or production environments

## ✅ 5. Access Logging (Optional)
Enables tracking of who accessed the data and when
![Screenshot 2025-06-04 145257](https://github.com/user-attachments/assets/cad2f8c6-f401-4a4b-baaa-fb7e43ae6694)

![Screenshot 2025-06-04 145335](https://github.com/user-attachments/assets/39a40aa3-0917-47b4-94ce-e8a751d3876e)


## 🔐 Data Encryption in AWS

### 📘 What is Data Encryption?

Data encryption is the process of converting readable data into an unreadable format to prevent unauthorized access. In AWS, encryption can be applied:
- **At rest** (when stored in S3, RDS, EBS, etc.)
- **In transit** (when data is moving between services or users)

---

### 🔒 Why Use Encryption?

- ✅ Protects sensitive data (personal info, passwords, business files)
- ✅ Required for compliance (like GDPR, HIPAA, ISO 27001)
- ✅ Prevents exposure if unauthorized access or data leaks occur

---

## ✅ Types of Encryption in AWS S3

| Method         | Description                                                    | Who Manages the Key?          |
|----------------|----------------------------------------------------------------|-------------------------------|
| **SSE-S3**     | Server-side encryption using Amazon S3–managed keys            | AWS manages the keys          |
| **SSE-KMS**    | Encryption using AWS Key Management Service (KMS)              | You manage keys via AWS KMS   |
| **SSE-C**      | Encryption with customer-provided keys                         | You fully manage the keys     |
| **Client-side**| Encrypt the data before uploading to AWS                       | You manage all encryption     |

---

## 🛠️ What We Used: SSE-S3

For this project, we enabled **Server-Side Encryption with S3-managed keys (SSE-S3)** on our bucket.

### Steps to Enable SSE-S3:
1. Go to AWS S3 Console
2. Select your bucket (e.g., `my-secure-bucket`)
3. Click **Properties**
4. Scroll to **Default Encryption**
5. Choose **"Enable" → SSE-S3**
6. Save changes
![Screenshot 2025-06-04 145257](https://github.com/user-attachments/assets/dab6cea0-29b8-41cf-b050-1487a565d28e)

Now all objects uploaded to this bucket will be encrypted **automatically at rest**.

---
