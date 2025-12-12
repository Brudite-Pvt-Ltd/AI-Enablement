# Amazon EC2 (Elastic Compute Cloud)

Amazon EC2 provides virtual servers (instances) in the cloud. It's like renting a computer in AWS data centers.

## Key Concepts

**Instance**: A virtual machine (server)
- Has CPU, memory (RAM), storage
- Can run any operating system

**Instance Type**: The size/capability of an instance
- `t2.micro`: Small, suitable for learning (free tier eligible)
- `t2.small`: Slightly larger
- `m5.large`: More powerful

**AMI (Amazon Machine Image)**: A template for an instance
- Contains operating system and pre-installed software
- Example: Ubuntu 22.04 LTS AMI

**Security Group**: A firewall for your instance
- Controls incoming and outgoing traffic

## Launching an EC2 Instance

1. **Go to EC2 Service**
   - In AWS Management Console, search for "EC2"
   - Click "EC2"

2. **Launch Instance**
   - Click "Instances" in the left sidebar
   - Click "Launch instances" button

3. **Name Your Instance**
   - Enter a name (e.g., "my-first-server")

4. **Choose an AMI**
   - Select "Ubuntu" (popular and free tier eligible)
   - Choose "Ubuntu Server 22.04 LTS"
   - Click "Select"

5. **Choose Instance Type**
   - Select "t2.micro" (free tier eligible)
   - Click "Next"

6. **Configure Instance**
   - Use default settings
   - Click "Next"

7. **Add Storage**
   - Default 8 GB is fine
   - Click "Next"

8. **Add Tags** (Optional)
   - Tags help organize resources
   - Click "Next"

9. **Configure Security Group**
   - This is like a firewall
   - Create new security group
   - Name: "my-server-sg"
   - Add rules to allow traffic:
     - SSH (port 22) from your IP
     - HTTP (port 80) from anywhere
     - HTTPS (port 443) from anywhere
   - Click "Next"

10. **Review and Launch**
    - Review all settings
    - Click "Launch"

11. **Create Key Pair**
    - Create new key pair
    - Name: "my-ec2-key"
    - Click "Create key pair"
    - Your `.pem` file will download
    - **IMPORTANT**: Save this file securely; you'll need it to connect

12. **Instance Launching**
    - Click "Launch instances"
    - Your instance is being created

## Connecting to Your EC2 Instance

### From macOS/Linux

1. **Open Terminal**
2. **Navigate to key file directory**
   ```bash
   cd ~/Downloads  # or wherever you saved the .pem file
   ```

3. **Change permissions**
   ```bash
   chmod 400 my-ec2-key.pem
   ```

4. **Connect via SSH**
   ```bash
   ssh -i my-ec2-key.pem ubuntu@your-instance-public-ip
   ```

### From Windows

1. **Using PuTTY** (download separately)
   - Convert `.pem` to `.ppk` format using PuTTYgen
   - Open PuTTY
   - Enter instance public IP
   - In "SSH > Auth", select your `.ppk` file
   - Click "Open"

2. **Using Git Bash or PowerShell** (if you have OpenSSH)
   ```bash
   ssh -i my-ec2-key.pem ubuntu@your-instance-public-ip
   ```

## Finding Your Instance's Public IP

1. Go to EC2 Dashboard
2. Click "Instances"
3. Select your instance
4. Look for "Public IPv4 address" in the details panel
5. Copy and use this IP to connect

## Basic EC2 Management

### Stopping an Instance
(Costs pause but storage persists):
1. Go to EC2 Dashboard
2. Select your instance
3. Click "Instance State" > "Stop"

### Starting a Stopped Instance
1. Go to EC2 Dashboard
2. Select your instance
3. Click "Instance State" > "Start"

### Terminating an Instance
(Deletes everything):
1. Go to EC2 Dashboard
2. Select your instance
3. Click "Instance State" > "Terminate"
4. **IMPORTANT**: You cannot recover terminated instances

### Getting Instance Details

- Click on instance name in EC2 Dashboard
- View public IP, security groups, and other details

## Understanding Security Groups

Security groups act as virtual firewalls for your instances.

### Common Security Group Rules

| Protocol | Port | Source | Purpose |
|----------|------|--------|---------|
| SSH | 22 | Your IP | Remote access |
| HTTP | 80 | 0.0.0.0/0 | Web traffic |
| HTTPS | 443 | 0.0.0.0/0 | Secure web traffic |
| Custom TCP | 3000 | 0.0.0.0/0 | Application server |
| Custom TCP | 5432 | Your IP | PostgreSQL database |

### Modifying Security Groups

1. Select your instance
2. Click "Security" tab
3. Click on the security group
4. Click "Edit inbound rules"
5. Add or remove rules as needed
6. Click "Save rules"

## EC2 Use Cases for Beginners

1. **Test your applications**
2. **Run a web server**
3. **Learn Linux commands**
4. **Build and deploy applications**
5. **Practice server administration**

## Cost Optimization Tips

1. **Use t2.micro** (free tier eligible) for learning
2. **Stop instances** when not in use (don't terminate if you want to keep data)
3. **Set up CloudWatch alarms** to monitor costs
4. **Use Auto Scaling** to manage resources automatically
5. **Review and delete** unused instances regularly

## Elastic IPs

Elastic IPs are static public IP addresses that persist even after stopping/starting an instance.

### Allocating an Elastic IP

1. In EC2 Dashboard, click "Elastic IPs"
2. Click "Allocate Elastic IP address"
3. Select "VPC"
4. Click "Allocate"
5. Select the newly created IP
6. Click "Associate Elastic IP address"
7. Select your instance and private IP
8. Click "Associate"

**Note**: Unused Elastic IPs incur charges, so deallocate them if not needed.
