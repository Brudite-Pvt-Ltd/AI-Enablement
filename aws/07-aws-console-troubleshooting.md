# AWS Management Console and Troubleshooting

## AWS Management Console Overview

The AWS Management Console is your central hub for managing all AWS services.

### Console Layout

1. **Top Navigation Bar**
   - AWS logo (click to go home)
   - Search bar (find services)
   - Region selector (top-right)
   - Account menu (top-right)

2. **Service Menu** (Left sidebar)
   - Browse all AWS services by category
   - Favorites section at top
   - Search within services

3. **Main Dashboard**
   - Service shortcuts
   - Recent services
   - Resource summary

### Accessing Services

**Method 1: Search Bar**
1. Click the search bar at the top
2. Type service name (e.g., "S3")
3. Click the service in results

**Method 2: Service Menu**
1. Click "Services" (top-left)
2. Browse by category
3. Click desired service

**Method 3: Recent Services**
1. Find service in recent list on home dashboard
2. Click to access

### Changing Regions

Regions are geographic locations where AWS resources run.

1. **Click region selector** (top-right, next to account menu)
2. **Select your region** (e.g., us-east-1, ap-south-1)
3. **Page reloads** with new region

**Note**: Different regions have different available services and pricing.

### Understanding the URL Structure

AWS Console URLs follow this pattern:
```
https://console.aws.amazon.com/s3/?region=us-east-1
```

- `s3` = Service
- `region=us-east-1` = Selected region

### Browser Tabs and Window Management

- **Open in new tab**: Middle-click any service link
- **Bookmark frequently used services**: Add to browser bookmarks
- **Use multiple windows**: One for each AWS account (if you have multiple)

---

## Common Issues and Solutions

### Issue 1: Cannot Find Service

**Problem**: Can't locate a specific AWS service in the console.

**Solution**:
1. Use the search bar (faster than browsing menu)
2. Type partial service name
3. Look at "AWS service" category results
4. Bookmark frequently used services for quick access

### Issue 2: Service Not Available in Region

**Problem**: Service shows as unavailable in your region.

**Solution**:
1. Check AWS region status page
2. Switch to a different region
3. Some services are only in specific regions
4. Common regions: us-east-1, us-west-2, eu-west-1, ap-south-1

### Issue 3: Permission Denied Error

**Problem**: Getting "Access Denied" or "Not Authorized" error.

**Cause**: Your IAM user doesn't have required permissions.

**Solution**:
```
1. Sign in with account that has admin access
2. Go to IAM Dashboard
3. Click "Users"
4. Select your user
5. Click "Add permissions"
6. Select "Attach existing policies directly"
7. Search for required service policy (e.g., "AmazonS3FullAccess")
8. Check the policy
9. Click "Next"
10. Click "Add permissions"
```

### Issue 4: Unexpected Charges

**Problem**: Getting unexpected AWS bills.

**Causes**:
- Free tier expired
- Leaving resources running (EC2, RDS)
- Data transfer charges
- Storage costs

**Solution**:
1. Go to **Billing Dashboard**
2. Click **Costs and Usage**
3. Review charges by service
4. Delete unused resources
5. Set up **Cost Budgets** for alerts

**Prevention**:
1. Stop (don't terminate) instances when not in use
2. Delete test databases
3. Monitor free tier usage
4. Set billing alerts

### Issue 5: Can't Connect to EC2 Instance

**Problem**: SSH connection times out or refuses connection.

**Causes**:
- Security group not configured
- Wrong key pair
- Instance not running
- Public IP not assigned

**Solution**:
1. **Verify instance is running**
   - Go to EC2 Dashboard
   - Check instance status shows "running"

2. **Verify security group allows SSH**
   - Click instance
   - Go to "Security" tab
   - Check port 22 is allowed from your IP

3. **Verify key pair**
   - Confirm you have the correct .pem file
   - Check file permissions: `chmod 400 key.pem`

4. **Check public IP**
   - Ensure instance has public IP assigned
   - Copy the "Public IPv4 address"

5. **Test connection**
   ```bash
   ssh -i key.pem ubuntu@your-public-ip
   ```

### Issue 6: S3 Bucket Access Denied

**Problem**: Can't upload files or access S3 bucket.

**Cause**: Bucket policy or permissions issue.

**Solution**:
1. **Check bucket policy**
   - Go to S3 bucket
   - Click "Permissions" tab
   - Review bucket policy

2. **Check object permissions**
   - Click on file
   - Go to "Permissions" tab
   - Ensure your IAM user has access

3. **Public access blocked**
   - If trying to access publicly
   - Click "Permissions" tab
   - Check "Block public access" settings
   - Disable if needed for public access

### Issue 7: Lambda Function Timeout

**Problem**: Lambda function fails with timeout error.

**Cause**: Function takes longer than timeout setting.

**Solution**:
1. Go to Lambda function
2. Click "Configuration" tab
3. Click "General configuration"
4. Click "Edit"
5. Increase "Timeout" value (max 15 minutes)
6. Click "Save"

**Prevention**:
- Optimize your code
- Use layers for dependencies
- Monitor with CloudWatch logs

### Issue 8: RDS Database Unavailable

**Problem**: Can't connect to RDS database.

**Causes**:
- Database not fully created
- Security group blocking connection
- Database subnet group misconfigured

**Solution**:
1. **Check database status**
   - Go to RDS Dashboard
   - Verify status shows "available"
   - Wait if showing "creating" or "modifying"

2. **Verify security group**
   - Click on database
   - Go to "Connectivity" section
   - Check security group allows your IP on database port

3. **Test connectivity**
   ```bash
   # For PostgreSQL
   psql -h your-rds-endpoint -U admin -d mydb
   
   # For MySQL
   mysql -h your-rds-endpoint -u admin -p
   ```

### Issue 9: DynamoDB Item Not Found

**Problem**: Query returns no results even though item exists.

**Cause**: Wrong partition key or query format.

**Solution**:
1. **Verify partition key value**
   - Make sure you're using exact same key
   - Check for spaces or case sensitivity

2. **Use scan instead**
   - If unsure about query syntax
   - Use "Scan" to see all items

3. **Check attribute names**
   - Verify exact attribute names match
   - DynamoDB is case-sensitive

### Issue 10: CloudWatch Logs Empty

**Problem**: Lambda/EC2 logs not appearing in CloudWatch.

**Cause**: IAM role doesn't have logging permissions.

**Solution**:
1. Check IAM role has `CloudWatchLogsFullAccess`
2. Verify log group is created
3. Check function actually writes logs
4. Use `print()` statements in code (Python)

---

## Best Practices Checklist

- [ ] **Security**: Use IAM users, not root account
- [ ] **Billing**: Set up cost alerts and budgets
- [ ] **Backup**: Enable automated backups for databases
- [ ] **Monitoring**: Use CloudWatch for resource monitoring
- [ ] **Tagging**: Tag resources for organization
- [ ] **Documentation**: Document your architecture
- [ ] **Testing**: Test in development before production
- [ ] **Updates**: Keep OS and software updated
- [ ] **Cleanup**: Delete unused resources regularly
- [ ] **Access**: Review permissions regularly

## Key Shortcuts and Tips

| Task | Steps |
|------|-------|
| Open service quickly | Press `/` to focus search bar |
| Copy resource ID | Hover and click copy icon |
| Return to home | Click AWS logo |
| View service status | Click region > Service Health |
| Access previous pages | Use browser back button |
| Open in new tab | Ctrl/Cmd + Click link |

## Support and Resources

### Getting Help

1. **AWS Documentation**: [docs.aws.amazon.com](https://docs.aws.amazon.com)
2. **AWS Support Center**: Available in console (top-right menu)
3. **AWS Forums**: Community support
4. **Stack Overflow**: Tag questions with AWS service name
5. **AWS Whitepapers**: In-depth guides on AWS services

### Support Plans

- **Basic**: Free, community support only
- **Developer**: $29+/month, email support
- **Business**: $100+/month, phone + email support
- **Enterprise**: Custom pricing, dedicated support

---

## Cleanup and Cost Management

### Deleting Resources Before Moving On

**Always remember to delete these to avoid charges**:

1. **EC2 Instances**
   - Select instance → Instance State → Terminate

2. **RDS Databases**
   - Select database → Delete → Confirm

3. **DynamoDB Tables**
   - Select table → Delete

4. **S3 Buckets** (keep only if you need data)
   - Empty bucket first
   - Delete bucket

5. **Lambda Functions** (if not using)
   - Select function → Delete

### Cost Monitoring Workflow

1. Visit Billing Dashboard daily during learning
2. Check "Cost and Usage" tab
3. Look for unexpected charges
4. Set monthly budget ($5-10 for learning)
5. Delete test resources daily
