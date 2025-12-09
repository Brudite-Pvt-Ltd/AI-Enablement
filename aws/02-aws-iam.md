# Identity and Access Management (IAM)

IAM (Identity and Access Management) allows you to manage who has access to AWS resources and what they can do.

## Why IAM Matters?

- **Security**: Use root account only for account creation, use IAM users for daily work
- **Access Control**: Grant specific permissions to different users
- **Audit Trail**: Track who did what in your AWS account

## Understanding IAM Users vs Root Account

**Root Account** (Account Owner):
- Has complete access to all AWS services
- Should only be used for account setup
- Never share root account credentials

**IAM User** (Individual Account):
- Created by root account
- Has specific permissions assigned
- Safer for daily development work

## Creating an IAM User

1. **Go to IAM Dashboard**
   - In AWS Management Console, search for "IAM"
   - Click on "IAM"

2. **Create User**
   - In the left sidebar, click "Users"
   - Click "Create user"
   - Enter username (e.g., "developer-user")
   - Click "Next"

3. **Set Permissions**
   - Select "Attach policies directly"
   - Search for "AdministratorAccess" (for learning purposes)
   - Check the box next to "AdministratorAccess"
   - Click "Next"

4. **Review and Create**
   - Review the details
   - Click "Create user"

5. **Create Access Key** (For Programmatic Access)
   - Click on the newly created user
   - Go to "Security credentials" tab
   - Click "Create access key"
   - Choose "Command Line Interface (CLI)"
   - Check the acknowledgment box
   - Click "Next"
   - Copy the Access Key ID and Secret Access Key
   - **IMPORTANT**: Save these in a secure location; you won't be able to see them again
   - Click "Done"

## Understanding Permissions and Policies

A policy is a document that grants or denies permissions:

**Managed Policies**: Pre-created policies by AWS
- Example: AdministratorAccess, PowerUserAccess, ReadOnlyAccess

**Custom Policies**: Create your own permissions
- Example: Allow user to only access S3 bucket named "my-app-data"

### Example Policy (JSON)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-app-data/*"
    }
  ]
}
```

## IAM Best Practices

1. **Don't use root account** for daily tasks
2. **Create individual IAM users** for each person
3. **Use strong passwords** for IAM users
4. **Enable MFA** (Multi-Factor Authentication) for important accounts
5. **Regularly review permissions** and remove unnecessary access
6. **Never hardcode access keys** in your code; use environment variables or AWS SDK

## IAM Roles

IAM roles are used to grant permissions to AWS services to act on your behalf.

### Creating an IAM Role

1. In IAM Dashboard, click "Roles"
2. Click "Create role"
3. Select trusted entity type (e.g., "AWS service")
4. Select the service (e.g., "Lambda")
5. Attach policies
6. Review and create

### Use Cases for Roles

- **Lambda functions** accessing S3
- **EC2 instances** accessing databases
- **Services** communicating with each other
