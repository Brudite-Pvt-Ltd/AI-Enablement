# AWS Lambda

AWS Lambda is serverless computing. You upload code and AWS manages the servers. You only pay for the compute time you use.

## Key Concepts

**Function**: Your code that runs on Lambda
- Written in Python, Node.js, Java, Go, etc.
- Triggered by events

**Trigger**: An event that causes your function to run
- S3 file upload
- API call
- Scheduled time

**Execution Role**: IAM role that gives your function permissions

## Creating Your First Lambda Function

1. **Go to Lambda Service**
   - In AWS Management Console, search for "Lambda"
   - Click "Lambda"

2. **Create Function**
   - Click "Create function" button

3. **Configure Function**
   - **Function name**: `my-first-lambda` (or any name)
   - **Runtime**: `Python 3.12` (or latest)
   - **Role**: Create new role with basic Lambda permissions
   - Click "Create function"

4. **Write Code**
   - You'll see a code editor
   - Replace the default code with:
   ```python
   def lambda_handler(event, context):
       return {
           'statusCode': 200,
           'body': 'Hello from Lambda!'
       }
   ```

5. **Save and Test**
   - Click "Deploy" button
   - Click "Test" button
   - Select "Create new event"
   - Name: `test-event`
   - Click "Create"
   - Click "Test" again
   - You'll see the output

## Understanding Lambda Events and Context

**event**: Data passed to your function
```python
# Example: API call with JSON data
{
    "name": "John",
    "age": 30
}
```

**context**: Information about the execution
```python
# Access context information
context.function_name  # Name of function
context.aws_request_id  # Unique request ID
context.log_group_name  # CloudWatch log group
```

## Creating a Lambda Function with Input

```python
def lambda_handler(event, context):
    # Get data from event
    name = event.get('name', 'Guest')
    
    # Process
    message = f'Hello, {name}!'
    
    # Return response
    return {
        'statusCode': 200,
        'body': message
    }
```

**Test it**:
1. Click "Test"
2. Modify the test event to:
   ```json
   {
       "name": "Alice"
   }
   ```
3. Click "Test"
4. You'll see: `'Hello, Alice!'`

## Lambda Triggers

### API Gateway (Trigger from HTTP requests)

1. Click "Add trigger"
2. Select "API Gateway"
3. Select "Create new API"
4. Choose "HTTP API"
5. Click "Add"
6. You'll get an API endpoint to call your function

### S3 (Trigger from file uploads)

1. Click "Add trigger"
2. Select "S3"
3. Choose your bucket
4. Select event type: "All object creation events"
5. Click "Add"

### Scheduled (Trigger at specific times)

1. Click "Add trigger"
2. Select "EventBridge (CloudWatch Events)"
3. Create new rule
4. Enter schedule (e.g., "rate(1 hour)" or "cron(0 12 * * ? *)")
5. Click "Add"

## Lambda Best Practices

1. **Keep functions small** - One responsibility per function
2. **Use environment variables** for configuration
3. **Handle errors gracefully** with try-except blocks
4. **Set appropriate timeout** (default is 3 seconds)
5. **Monitor with CloudWatch** logs
6. **Use layers** for shared code and dependencies

## Working with Layers

Layers allow you to package libraries and custom code to use in Lambda functions.

### Creating a Layer

1. In Lambda Dashboard, click "Layers"
2. Click "Create layer"
3. Enter layer name
4. Upload a ZIP file containing dependencies
5. Select compatible runtimes
6. Click "Create"

### Using a Layer

1. Open your Lambda function
2. Scroll to "Layers"
3. Click "Add a layer"
4. Select your layer
5. Click "Add"

## Lambda Pricing

- **Free tier**: 1 million requests/month + 400,000 GB-seconds of compute
- **Paid**: $0.20 per 1 million requests + $0.0000166667 per GB-second

## Environment Variables

Store configuration without hardcoding:

```python
import os
import json

def lambda_handler(event, context):
    db_host = os.environ.get('DB_HOST')
    db_name = os.environ.get('DB_NAME')
    
    # Use variables
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': f'Connected to {db_name}'
        })
    }
```

**Set Environment Variables**:
1. Go to Lambda function
2. Click "Configuration" tab
3. Click "Environment variables"
4. Click "Edit"
5. Add key-value pairs
6. Click "Save"

## Handling Errors

```python
def lambda_handler(event, context):
    try:
        # Your code here
        name = event['name']
        return {
            'statusCode': 200,
            'body': f'Hello {name}'
        }
    except KeyError:
        return {
            'statusCode': 400,
            'body': 'Missing required field: name'
        }
    except Exception as e:
        print(f'Error: {str(e)}')
        return {
            'statusCode': 500,
            'body': 'Internal server error'
        }
```

## Lambda Use Cases for Beginners

1. **Automate file processing** when uploaded to S3
2. **Build serverless APIs** with API Gateway
3. **Send notifications** when events occur
4. **Process data** on a schedule
5. **Transform data** between services
6. **Create webhooks** for external services
