# Amazon S3 (Simple Storage Service)

Amazon S3 is object storage for files, images, videos, backups, and static websites.

## Key Concepts

**Bucket**: A container for objects (like a folder in your computer)
- Bucket names must be globally unique
- Example: `my-app-data-bucket-2025`

**Object**: A file stored in S3
- Can be any type: images, documents, videos, etc.
- Maximum file size: 5 TB

**Key**: The path/name of an object in a bucket
- Example: `images/profile-pic.jpg`

## Creating an S3 Bucket

1. **Go to S3 Service**
   - In AWS Management Console, search for "S3"
   - Click "S3"

2. **Create Bucket**
   - Click "Create bucket" button

3. **Configure Bucket**
   - **Bucket name**: Enter a unique name (lowercase, no spaces)
     - Example: `my-learning-bucket-2025`
   - **Region**: Select your closest region
   - **Block public access**: Keep enabled (unless you want public access)
   - Click "Create bucket"

4. **Your bucket is created!**
   - You'll see it in your bucket list

## Uploading Files to S3

1. **Click on your bucket name**

2. **Upload Files**
   - Click "Upload" button
   - Click "Add files" or "Add folder"
   - Select files from your computer
   - Click "Upload"

3. **Verify Upload**
   - You'll see the files listed in the bucket
   - Status shows "Uploaded"

## Accessing Files from S3

**Getting the File URL**:
1. Click on the file name in your bucket
2. Copy the "Object URL" (at the bottom)
3. This URL allows you to access the file from anywhere

**Making Files Public**:
1. Click on the file
2. Go to "Permissions" tab
3. To make public, you need to modify bucket policy (advanced)
4. For learning, use pre-signed URLs instead (see AWS documentation)

## S3 Storage Classes (Simplified)

**Standard**: Frequently accessed files
- Lowest latency, highest cost
- Use for: Website files, active data

**Infrequent Access**: Less frequently accessed files
- Lower cost, slight retrieval fee
- Use for: Backups, archives

**Glacier**: Long-term archival
- Lowest cost, slow retrieval
- Use for: Compliance, historical data

## S3 Use Cases for Beginners

1. **Store application files**
2. **Host static websites**
3. **Backup important files**
4. **Store user-uploaded content**
5. **Distribute files globally**

## S3 Bucket Policies

Bucket policies control access to your S3 bucket:

### Example: Make Bucket Public for Reading

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### Example: Allow Specific IAM User

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/myuser"
      },
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

## S3 Best Practices

1. **Enable versioning** to keep file history
2. **Use bucket encryption** to secure data
3. **Set up lifecycle policies** to automatically archive old files
4. **Monitor access** with logging and CloudTrail
5. **Use appropriate storage class** based on access patterns
6. **Delete unused buckets** to avoid costs

## Managing Your S3 Resources

### Deleting Objects
1. Click on object
2. Click "Delete"
3. Confirm deletion

### Deleting a Bucket
1. Click on bucket
2. Click "Delete"
3. Enter bucket name to confirm
4. **Note**: Bucket must be empty before deletion

### Downloading Objects
1. Click on object
2. Click "Download"
3. File saves to your computer
