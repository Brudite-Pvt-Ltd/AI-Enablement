# AWS Beginner Guide - File Index

This is the index for the AWS Beginner Guide. The original document has been broken down into 7 smaller, focused sections for easier reading and reference.

## Files Overview

### 1. **01-aws-account-setup.md**
Getting started with AWS:
- Account creation prerequisites
- Step-by-step account creation process
- AWS Free Tier explanation and limitations
- Setting up billing alerts to monitor costs
- Understanding basic AWS concepts (regions, availability zones, resources)
- Navigating the AWS Management Console
- Best practices for beginners

### 2. **02-aws-iam.md**
Identity and Access Management (IAM):
- Why IAM is important for security
- Root account vs IAM users
- Creating IAM users step-by-step
- Creating access keys for programmatic access
- Understanding permissions and policies
- Managed vs custom policies
- IAM roles for service-to-service access
- IAM best practices

### 3. **03-aws-s3.md**
Amazon Simple Storage Service (S3):
- Key concepts: buckets, objects, keys
- Creating S3 buckets
- Uploading and accessing files
- File URLs and public access
- Storage classes (Standard, Infrequent Access, Glacier)
- Bucket policies and access control
- S3 best practices
- Managing S3 resources

### 4. **04-aws-ec2.md**
Elastic Compute Cloud (EC2):
- EC2 concepts: instances, instance types, AMIs, security groups
- Step-by-step instance launching
- Connecting to EC2 instances (macOS, Linux, Windows)
- Managing instances (stop, start, terminate)
- Finding and using instance public IPs
- Understanding security groups
- Modifying security group rules
- Elastic IPs for static addresses
- Cost optimization tips

### 5. **05-aws-lambda.md**
AWS Lambda (Serverless Computing):
- Lambda concepts: functions, triggers, execution roles
- Creating your first Lambda function
- Understanding events and context
- Writing Lambda functions with input
- Common triggers (API Gateway, S3, EventBridge)
- Lambda best practices
- Working with layers
- Environment variables
- Error handling
- Use cases for serverless applications

### 6. **06-aws-databases.md**
Relational and NoSQL Databases:
- **RDS (Relational Database Service)**:
  - Supported database engines (MySQL, PostgreSQL, etc.)
  - Creating RDS instances
  - Connecting to databases
  - RDS best practices

- **DynamoDB (NoSQL)**:
  - DynamoDB concepts: tables, items, attributes
  - Creating DynamoDB tables
  - Adding and querying items
  - Updating and deleting items
  - DynamoDB pricing models
  - Best practices

- **RDS vs DynamoDB comparison** for choosing right database

### 7. **07-aws-console-troubleshooting.md**
Console navigation and troubleshooting:
- AWS Management Console layout
- Accessing services (3 methods)
- Changing regions
- Understanding console URLs
- Browser management tips
- **10 common issues and solutions**:
  1. Cannot find service
  2. Service not available in region
  3. Permission denied error
  4. Unexpected charges
  5. Can't connect to EC2
  6. S3 bucket access denied
  7. Lambda function timeout
  8. RDS database unavailable
  9. DynamoDB item not found
  10. CloudWatch logs empty
- Best practices checklist
- Key shortcuts and tips
- Support resources and getting help
- Cleanup and cost management

## How to Use This Guide

### For Complete Beginners
1. Start with **01-aws-account-setup.md** - Create your account
2. Read **02-aws-iam.md** - Set up security
3. Try **03-aws-s3.md** - Store your first file
4. Explore **05-aws-lambda.md** - Create serverless function
5. Reference **07-aws-console-troubleshooting.md** as needed

### For Learning Specific Services
- **Storage**: Jump to **03-aws-s3.md**
- **Compute**: Jump to **04-aws-ec2.md** or **05-aws-lambda.md**
- **Database**: Jump to **06-aws-databases.md**
- **Security**: Jump to **02-aws-iam.md**

### For Troubleshooting
- Go directly to **07-aws-console-troubleshooting.md**
- Find your issue
- Follow the solution steps

## Services Overview by File

| File | Service | Type | Difficulty |
|------|---------|------|------------|
| 01-aws-account-setup.md | AWS Core | Foundational | Beginner |
| 02-aws-iam.md | IAM | Security | Beginner |
| 03-aws-s3.md | S3 | Storage | Beginner |
| 04-aws-ec2.md | EC2 | Compute | Intermediate |
| 05-aws-lambda.md | Lambda | Serverless | Intermediate |
| 06-aws-databases.md | RDS & DynamoDB | Database | Intermediate |
| 07-aws-console-troubleshooting.md | All Services | Reference | Beginner-Advanced |

## Key Topics Summary

### Foundation (Must Read)
- Account creation and setup
- Understanding IAM and security
- AWS console navigation

### Core Services (Pick Based on Need)
- **Storage**: S3 for files and backups
- **Compute**: EC2 for virtual servers, Lambda for serverless
- **Database**: RDS for SQL, DynamoDB for NoSQL

### Ongoing Reference
- Console navigation tips
- Troubleshooting common issues
- Cost management and monitoring

## Free Tier Reminders

**Always remember**:
- Free tier expires after 12 months
- Delete test resources daily
- Monitor costs in Billing Dashboard
- Set monthly budget alerts ($5-10)
- Services included:
  - EC2: 750 hours/month (t2.micro)
  - S3: 5 GB storage
  - Lambda: 1 million requests/month
  - RDS: 750 hours/month (db.t2.micro)
  - DynamoDB: 25 GB storage

## Next Steps After Learning

1. **Build a project**: Combine S3 + Lambda + API Gateway
2. **Deploy application**: Use EC2 or Lambda with RDS
3. **Automate tasks**: Use Lambda with EventBridge
4. **Monitor costs**: Set up CloudWatch alarms
5. **Practice security**: Review IAM policies regularly
6. **Explore more services**: CloudFront, RDS, SNS, SQS, etc.

---

**Last Updated**: 2025
**AWS Console Version**: Current
**Services Covered**: S3, EC2, Lambda, RDS, DynamoDB, IAM
