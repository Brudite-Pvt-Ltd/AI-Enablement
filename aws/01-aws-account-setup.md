# AWS Account Setup and Basics

## AWS Account Creation

### Prerequisites

- Valid email address
- Credit/debit card for billing (required even for free tier)
- Phone number for verification
- Valid home address

### Step-by-Step Account Creation

1. **Visit AWS Homepage**
   - Go to [aws.amazon.com](https://aws.amazon.com)
   - Click "Create an AWS Account" button (top right)

2. **Enter Email and Password**
   - Provide a valid email address
   - Create a strong password
   - Re-enter password to confirm
   - Click "I agree to the AWS Customer Agreement..."
   - Click "Create account with email"

3. **Enter Contact Information**
   - Select "Personal" or "Business" account type
   - Enter your full name
   - Enter your address
   - Enter your phone number
   - Click "Continue"

4. **Add Payment Method**
   - Enter your credit/debit card details
   - Cardholder name, card number, expiration date, CVV
   - AWS will charge a small amount (usually $1) to verify the card
   - This charge will be refunded within a few business days
   - Click "Secure Submit"

5. **Verify Phone Number**
   - Select your country code
   - Enter your phone number
   - Choose "Text message (SMS)" or "Voice call"
   - Click "Send OTP"
   - Enter the verification code received on your phone
   - Click "Verify OTP"

6. **Select Support Plan**
   - Choose "Basic Plan" (free) for learning
   - Click "Complete Sign Up"

7. **Confirmation**
   - You'll see a confirmation message
   - Click "Go to AWS Management Console"
   - Sign in with your email and password

### AWS Free Tier Explained

AWS offers a free tier for 12 months with limitations:

**Services Included in Free Tier**:
- **EC2**: 750 hours/month of t2.micro instances
- **S3**: 5 GB of storage
- **Lambda**: 1 million free requests per month
- **RDS**: 750 hours/month of db.t2.micro or db.t3.micro
- **DynamoDB**: 25 GB of storage

**Important Notes**:
- Free tier is limited to 12 months from account creation date
- Always monitor your usage to avoid unexpected charges
- Set up billing alerts to get notified when costs exceed a threshold

### Setting Up Billing Alerts

1. Go to AWS Management Console
2. Search for "Billing" in the search bar
3. Click "Manage your costs and usage"
4. In the left sidebar, click "Budgets"
5. Click "Create a budget"
6. Select "Cost budget"
7. Set a monthly budget amount (e.g., $10)
8. Configure alerts to notify you when usage exceeds your budget
9. Click "Create budget"

## Understanding AWS Basics

### What is AWS?

Amazon Web Services (AWS) is a cloud computing platform that provides:
- **Computing power** (EC2, Lambda)
- **Storage** (S3, EBS)
- **Databases** (RDS, DynamoDB)
- **Networking** (VPC, Route 53)
- **And much more...**

### Key Concepts

**Region**: A geographic location where AWS has data centers
- Example: us-east-1 (Virginia), eu-west-1 (Ireland), ap-south-1 (Mumbai)
- Choose a region close to your users for lower latency

**Availability Zone (AZ)**: A data center within a region
- Each region has multiple AZs for redundancy

**Service**: An individual AWS product
- Example: EC2 is a service, S3 is another service

**Resource**: An instance of a service
- Example: An EC2 instance is a resource

### Navigating the AWS Management Console

1. **Home Dashboard**: Shows service shortcuts and recent services
2. **Search Bar**: Find services by name
3. **Service Menu**: Browse all available services
4. **Account Menu**: Manage account settings and billing
5. **Region Selector**: Change the region (top right)

### Best Practices for Beginners

1. **Always use the free tier resources** to learn without incurring charges
2. **Choose the closest region** to your location
3. **Delete resources when not in use** to avoid unexpected charges
4. **Monitor your billing regularly** through the Billing Dashboard
5. **Use the AWS free tier limits** as your learning boundary
6. **Read documentation** for each service before creating resources
