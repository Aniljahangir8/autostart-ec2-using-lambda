# autostart-ec2-using-lambda
Auto Start EC2 instance using AWS Lambda

# First Step create a role and attach Ec2 Full Access

1. Go to IAM and Select Roles

2. Create Role

3. Common use cases: Select Lambda

4. Add permissions: select AmazonEC2FullAccess

5. Role name: Auto-Start-And-Stop-Ec2 and Click on Create Role.

# Second Step go to aws lambda and create a function

Name Function as Start-Ec2-Auto

1. Author from scratch

2. Function name: Start-Ec2-Auto

3. Runtime: Python 3.7

4. Architecture: x86_64

5. Click on Change default execution role: Use an existing role and select created role in IAM (Auto-Start-And-Stop-Ec2)

6. Paste the Following function Auto-Start-Ec2 in lambda_function.py

``` 
import boto3

def lambda_handler(event, context):
    # Initialize the EC2 client
    ec2 = boto3.client('ec2')

    # Start an EC2 instance
    def start_instance(instance_id):
        response = ec2.start_instances(InstanceIds=[instance_id])
        print(f"Started EC2 instance: {instance_id}")
        return response

    # Replace 'YOUR_INSTANCE_ID' with the ID of the EC2 instance you want to start
    instance_id = 'YOUR_INSTANCE_ID'

    # Start the EC2 instance
    start_instance(instance_id)
```

Paste your instance id in: YOUR_INSTANCE_ID

Click on Configuration and Edit it. In Timeout default vaule is 3 Seconds so make it 2 Mins.

# Same as above start function, create a stop function in lambda and name as Auto-Stop-Ec2
```
import boto3

def lambda_handler(event, context):
    # Initialize the EC2 client
    ec2 = boto3.client('ec2')

    # Stop an EC2 instance
    def stop_instance(instance_id):
        response = ec2.stop_instances(InstanceIds=[instance_id])
        print(f"Stopped EC2 instance: {instance_id}")
        return response

    # Replace 'YOUR_INSTANCE_ID' with the ID of the EC2 instance you want to stop
    instance_id = 'YOUR_INSTANCE_ID'

    # Stop the EC2 instance
    stop_instance(instance_id)

```

-------------------------------------------------------------


# Click on add trigger and select Event Bridge for Scheduling the task

1. EventBridge Rule and click on create rule.

2. Name: Start-Ec2-Auto

3. Select Schedule and Continue in EventBridge Scheduler

4. Select Recurring schedule and setup cron according UTC time.

5. Target detail: Lambda and Select Lambda Function, Click on NEXT and now EventBridge Scheduler is created.

