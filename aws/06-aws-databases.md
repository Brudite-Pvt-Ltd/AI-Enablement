# Amazon RDS and DynamoDB

AWS offers two main database options: RDS (relational) and DynamoDB (NoSQL).

## Amazon RDS (Relational Database Service)

RDS is a managed relational database service that handles backups, patches, and maintenance.

### Supported Database Engines

- **MySQL**: Open-source relational database
- **PostgreSQL**: Advanced open-source relational database
- **MariaDB**: MySQL alternative
- **Oracle**: Enterprise database
- **SQL Server**: Microsoft's database

### Creating an RDS Instance

1. **Go to RDS Service**
   - In AWS Management Console, search for "RDS"
   - Click "RDS"

2. **Create Database**
   - Click "Create database"

3. **Choose Engine**
   - Select your database engine (e.g., PostgreSQL)
   - Select version
   - Click "Continue"

4. **DB Instance Identifier**
   - Enter a unique name (e.g., "my-learning-db")

5. **Master Username and Password**
   - Username: `admin` (or your choice)
   - Password: Create a strong password
   - **IMPORTANT**: Save the password securely

6. **DB Instance Class**
   - Select `db.t2.micro` (free tier eligible)

7. **Storage**
   - Allocate storage (20 GB is default for free tier)
   - Enable "Storage autoscaling" if needed

8. **Connectivity**
   - VPC: Use default VPC
   - Publicly accessible: Yes (for learning, allows external access)

9. **Database Options**
   - Database name: `mydb` (optional)
   - Backup retention: 7 days

10. **Create Database**
    - Click "Create database"
    - Wait for instance to be created (5-10 minutes)

### Connecting to RDS Database

**Using MySQL/MariaDB Client**:
```bash
mysql -h your-rds-endpoint.aws.com -P 3306 -u admin -p
# Enter password when prompted
```

**Using PostgreSQL Client**:
```bash
psql -h your-rds-endpoint.aws.com -U admin -d mydb
# Enter password when prompted
```

**From Application Code**:
```python
import psycopg2

connection = psycopg2.connect(
    host="your-rds-endpoint.aws.com",
    database="mydb",
    user="admin",
    password="your-password"
)

cursor = connection.cursor()
cursor.execute("SELECT * FROM users")
results = cursor.fetchall()
cursor.close()
connection.close()
```

### RDS Best Practices

1. **Use security groups** to restrict access
2. **Enable backups** for data protection
3. **Use parameter groups** for configuration
4. **Monitor performance** with CloudWatch
5. **Regular maintenance windows** for updates
6. **Multi-AZ deployment** for high availability

---

## Amazon DynamoDB

DynamoDB is a fully managed NoSQL database that provides fast, predictable performance.

### Key Concepts

**Table**: A collection of items (like a spreadsheet)

**Item**: A single record (like a row in a spreadsheet)

**Attribute**: A single value within an item (like a cell)

**Primary Key**: Uniquely identifies each item
- Partition Key: Distributes data across partitions
- Sort Key: (Optional) Orders items within a partition

### Creating a DynamoDB Table

1. **Go to DynamoDB Service**
   - In AWS Management Console, search for "DynamoDB"
   - Click "DynamoDB"

2. **Create Table**
   - Click "Create table"

3. **Table Details**
   - **Table name**: `Users` (or your choice)
   - **Partition key**: `user_id` (String)
   - **Sort key**: (Optional) Leave empty for now
   - Click "Create"

4. **Table Created!**
   - Table is ready to use

### Adding Items to DynamoDB

1. **Open Your Table**
   - Click on table name

2. **Explore Items**
   - Click "Items" tab

3. **Add Item**
   - Click "Create item"
   - Enter attributes:
     - `user_id`: `user123`
     - `name`: `John`
     - `email`: `john@example.com`
   - Click "Create item"

4. **Add More Items**
   - Repeat to add multiple items

### Querying DynamoDB

**Using AWS Console**:
1. Click "Items" tab
2. Use the scan or query feature
3. Filter results as needed

**Using Python**:
```python
import boto3

# Create DynamoDB client
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Users')

# Get an item
response = table.get_item(
    Key={'user_id': 'user123'}
)
item = response['Item']
print(item)

# Query items
response = table.query(
    KeyConditionExpression='user_id = :uid',
    ExpressionAttributeValues={
        ':uid': 'user123'
    }
)
items = response['Items']

# Scan all items
response = table.scan()
items = response['Items']
```

### Updating Items in DynamoDB

**Using Python**:
```python
table.update_item(
    Key={'user_id': 'user123'},
    UpdateExpression='SET #email = :email',
    ExpressionAttributeNames={
        '#email': 'email'
    },
    ExpressionAttributeValues={
        ':email': 'newemail@example.com'
    }
)
```

### Deleting Items from DynamoDB

**Using Python**:
```python
table.delete_item(
    Key={'user_id': 'user123'}
)
```

### DynamoDB Pricing

- **On-demand**: Pay per request (good for unpredictable workloads)
- **Provisioned**: Fixed capacity (good for predictable workloads)
- **Free tier**: 25 GB of storage + request limits

### DynamoDB Best Practices

1. **Choose appropriate partition key** for even data distribution
2. **Use indexes** for efficient queries
3. **Enable point-in-time recovery** for backups
4. **Monitor with CloudWatch** for performance
5. **Use DynamoDB Streams** for real-time data changes
6. **Set TTL** (Time to Live) for automatic item expiration

---

## RDS vs DynamoDB Comparison

| Feature | RDS | DynamoDB |
|---------|-----|----------|
| Type | Relational SQL | NoSQL Document |
| Schema | Fixed schema | Flexible schema |
| Queries | Complex joins | Simple key-value |
| Scaling | Vertical scaling | Horizontal scaling |
| Cost | Predictable | Pay-per-request |
| Use Cases | Traditional apps | Web/mobile apps |

## Choosing Between RDS and DynamoDB

**Use RDS when**:
- You need complex queries and joins
- Your schema is well-defined
- You have relational data
- Example: Banking system, ERP

**Use DynamoDB when**:
- You need fast, simple queries
- Your schema may change
- You have unpredictable traffic
- Example: Real-time applications, user profiles
