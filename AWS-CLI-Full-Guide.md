# AWS CLI Comprehensive Notes and Setup Guide

This document consolidates mentor notes, previous chat suggestions, and screenshots into a **complete reference for AWS CLI usage**. It covers IAM, RDS, S3, CloudTrail, and other core services with commands, definitions, and documentation links.

---

## 1. IAM (Identity and Access Management)

IAM manages **users, groups, roles, and permissions**.

### Key Definitions
- **User:** An identity representing a person or application.
- **Group:** A collection of IAM users with common permissions.
- **Role:** An identity with temporary credentials, usually assumed by applications or AWS services.
- **Policy:** JSON document defining permissions.

### Create IAM Group & User
```bash
# Create a new IAM group
aws iam create-group --group-name myawsdemogroup

# Create a new IAM user
aws iam create-user --user-name myawsdemouser

# Add user to group
aws iam add-user-to-group --user-name myawsdemouser --group-name myawsdemogroup
```

### Attach Policy to Group
```bash
aws iam attach-group-policy \
  --group-name myawsdemogroup \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

### List Users, Groups, and Policies
```bash
aws iam list-users
aws iam list-groups
aws iam list-attached-group-policies --group-name myawsdemogroup
```

📌 **Best Practice:** Always follow the **least privilege principle**.

---

## 2. RDS (Relational Database Service)

Amazon RDS is a managed database service.

### Key Definitions
- **Instance:** A database environment.
- **Snapshot:** A point-in-time backup.
- **DB Parameter Group:** Configurations for DB engine.
- **DB Subnet Group:** Defines which subnets DB can use.

### Create PostgreSQL RDS Instance
```bash
aws rds create-db-instance \
  --db-instance-identifier mydemordsdatabase \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password <YourPassword> \
  --allocated-storage 20 \
  --backup-retention-period 7 \
  --publicly-accessible
```

### Check RDS Status
```bash
aws rds describe-db-instances --db-instance-identifier mydemordsdatabase
```

### Modify RDS Instance
```bash
aws rds modify-db-instance \
  --db-instance-identifier mydemordsdatabase \
  --allocated-storage 25 \
  --apply-immediately
```

### Delete RDS Instance
```bash
aws rds delete-db-instance \
  --db-instance-identifier mydemordsdatabase \
  --skip-final-snapshot
```

---

## 3. Snapshots & Backups

### Create and Manage Snapshots
```bash
# Manual snapshot
aws rds create-db-snapshot \
  --db-snapshot-identifier mydemoinstance-snapshot \
  --db-instance-identifier mydemordsdatabase

# List snapshots
aws rds describe-db-snapshots --db-instance-identifier mydemordsdatabase

# Restore from snapshot
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier mydemordsdatabase-restored \
  --db-snapshot-identifier mydemoinstance-snapshot
```

📌 Automated backups can be configured during DB creation.

---

## 4. S3 (Simple Storage Service)

Amazon S3 stores files and data.

### Key Definitions
- **Bucket:** A container for storing objects.
- **Object:** The actual file or data stored.
- **Storage Class:** Defines cost/performance (Standard, IA, Glacier).

### Common S3 Commands
```bash
# Create bucket
aws s3 mb s3://buvanesh-test-20250919

# List buckets
aws s3 ls

# Upload file
aws s3 cp file.txt s3://buvanesh-test-20250919/

# Sync local folder to bucket
aws s3 sync ./localfolder s3://buvanesh-test-20250919/

# List objects
aws s3 ls s3://buvanesh-test-20250919/

# Download file
aws s3 cp s3://buvanesh-test-20250919/file.txt ./

# Delete object
aws s3 rm s3://buvanesh-test-20250919/file.txt

# Delete bucket
aws s3 rb s3://buvanesh-test-20250919 --force
```

---

## 5. AWS CLI Configuration

### Configure AWS CLI
```bash
aws configure
```
Provide:
- AWS Access Key ID
- AWS Secret Access Key
- Default region (ap-south-1 for Mumbai)
- Default output format (json, text, table)

### Validate Configuration
```bash
cat ~/.aws/config
cat ~/.aws/credentials
```

---

## 6. CloudTrail Integration with S3

CloudTrail logs all AWS API activity.

### Enable CloudTrail Logging
```bash
# Create trail
aws cloudtrail create-trail \
  --name mytrail \
  --s3-bucket-name buvanesh-cloudtrail-20250923-01

# Start logging
aws cloudtrail start-logging --name mytrail

# Check status
aws cloudtrail get-trail-status --name mytrail
```

📌 Logs are stored under `AWSLogs/` folder in the S3 bucket.

---

## 7. Useful Commands Across Services

```bash
# IAM
aws iam list-users
aws iam list-groups
aws iam list-roles

# RDS
aws rds describe-db-instances
aws rds describe-db-snapshots

# S3
aws s3 ls
aws s3api list-buckets

# CloudTrail
aws cloudtrail describe-trails
```

---

## 8. Best Practices
- Enable **MFA** for root user.
- Use IAM roles instead of hardcoded keys.
- Rotate access keys regularly.
- Use **S3 versioning** for critical buckets.
- Monitor logs with CloudTrail.
- Delete unused IAM users and keys.

---

## 9. Screenshots Reference

Store screenshots in a `screenshots/` folder for clarity:
```
AWS-CLI.md
screenshots/
   iam-group-user.png
   rds-database.png
   snapshot-backup.png
   s3-bucket.png
   cloudtrail-logs.png
```

Include them in documentation like:
```markdown
![IAM Group and User](screenshots/iam-group-user.png)
```

---

## 10. AWS Documentation References
- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)
- [IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
- [RDS Documentation](https://docs.aws.amazon.com/rds/index.html)
- [S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
- [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- [AWS Security Best Practices](https://docs.aws.amazon.com/general/latest/gr/aws-security-best-practices.html)

---

✅ This `AWS-CLI.md` file now includes **definitions, complete CLI commands, best practices, screenshots references, and AWS documentation links**.



# AWS CLI with Screenshots

This guide contains AWS CLI commands and screenshots.

### Iam Create User Step1
![iam-create-user-step1.png](iam-create-user-step1.png)

### Iam Create User Step2
![iam-create-user-step2.png](iam-create-user-step2.png)

### Iam Create User Step3
![iam-create-user-step3.png](iam-create-user-step3.png)

### Iam Create Accesskey Step1
![iam-create-accesskey-step1.png](iam-create-accesskey-step1.png)

### Iam Create Accesskey Step2
![iam-create-accesskey-step2.png](iam-create-accesskey-step2.png)

### Iam Create Accesskey Step3
![iam-create-accesskey-step3.png](iam-create-accesskey-step3.png)

### Iam Create Accesskey Step4
![iam-create-accesskey-step4.png](iam-create-accesskey-step4.png)

### Iam Accesskey Created
![iam-accesskey-created.png](iam-accesskey-created.png)

### Ec2 Instance Running
![ec2-instance-running.png](ec2-instance-running.png)

### Ec2 Instance Terminate Step1
![ec2-instance-terminate-step1.png](ec2-instance-terminate-step1.png)

### Ec2 Instance Terminate Step2
![ec2-instance-terminate-step2.png](ec2-instance-terminate-step2.png)

### S3 List Buckets
![s3-list-buckets.png](s3-list-buckets.png)

### S3 Bucket Objects
![s3-bucket-objects.png](s3-bucket-objects.png)

### Rds Database Dashboard
![rds-database-dashboard.png](rds-database-dashboard.png)

### Rds Database Snapshot
![rds-database-snapshot.png](rds-database-snapshot.png)

### Cloudtrail Dashboard
![cloudtrail-dashboard.png](cloudtrail-dashboard.png)

### Cloudtrail Logs S3
![cloudtrail-logs-s3.png](cloudtrail-logs-s3.png)

### Iam Group Overview
![iam-group-overview.png](iam-group-overview.png)

### Iam Policy Attach
![iam-policy-attach.png](iam-policy-attach.png)

