# Serverless File Processing and Notification System using AWS
**Automatically process files (e.g., images) uploaded to an S3 bucket using AWS Lambda, and send notifications (e.g., email) via SNS — all without managing servers.**



**Architecture Diagram**
Flow:

User uploads file →S3 triggers →Lambda processes file →SNS sends notification

**AWS Services Used & Their Roles**

📘 **Amazon S3 (Simple Storage Service):**
Amazon S3 is a scalable object storage service that allows you to store and retrieve any amount of data at any time from anywhere on the web.

📘 **AWS Lambda:**
AWS Lambda is a serverless compute service that runs your code in response to events without provisioning or managing servers.

📘 **Amazon SNS (Simple Notification Service):**
Amazon SNS is a fully managed messaging service for sending messages via email, SMS, or to other AWS services using topics.

📘 **AWS IAM (Identity and Access Management):**
IAM enables secure access control to AWS services and resources using roles, users, and policies.

📘  **Amazon CloudWatch:**
Amazon CloudWatch is a monitoring and observability service that provides logs, metrics, and alerts for AWS resources and applications.

| **Service**           | **Purpose in Project**               | **Main Benefit**                          |
| --------------------- | ------------------------------------ | ----------------------------------------- |
| **Amazon S3**         | Store and trigger on file upload     | Durable, scalable object storage          |
| **AWS Lambda**        | Execute Python code on S3 trigger    | No server management, pay-per-use         |
| **Amazon SNS**        | Send email/SMS notifications         | Real-time alerts, decoupled communication |
| **AWS IAM**           | Secure permissions for Lambda        | Fine-grained access control               |
| **Amazon CloudWatch** | Log execution and errors from Lambda | Observability and troubleshooting         |


**Step-by-Step Implementation**

✅ Step 1: Create an S3 Bucket
Go to S3 → **Create bucket**

Name: image-processing-bucket-123

Keep other settings default (disable public access)

Click **Create bucket**

✅ Step 2: Create SNS Topic and Subscription
Go to SNS → Topics → **Create topic**

Type: Standard

Name: file-processing-topic

After creation, click **Create subscription**

Protocol: Email

Endpoint: **Your email address**

Confirm email via inbox

✅ Step 3: Create IAM Role for Lambda
Go to IAM → Roles → **Create role**

Trusted entity: AWS service

Use case: Lambda

Attach permissions:

**AWSLambdaBasicExecutionRole (for CloudWatch logs)**

**AmazonS3ReadOnlyAccess**

**AmazonSNSFullAccess (or custom inline policy for sns:Publish)**

Name: LambdaS3SNSExecutionRole

Click **Create role**

✅ Step 4: Create Lambda Function
Go to Lambda → **Create function**

Name: processFileLambda

Runtime: Python 3.10

Role: **Use existing role** → Select LambdaS3SNSExecutionRole

✅ Step 5: Add Code to Lambda Function
python

✅ Step 6: Add Environment Variable to Lambda
Go to Configuration → Environment variables

Add:

Key: SNS_TOPIC_ARN

Value: ARN of your SNS topic

✅ Step 7: Set S3 Trigger for Lambda
Go to Lambda → **Triggers**

Add trigger:

Service: **S3**

Bucket: image-processing-bucket-123

Event type: PUT (Object created)

Optionally set suffix filter (e.g., .jpg, .png)

✅ Step 8: Test the System
Upload a file to S3 via console or AWS CLI

Lambda processes file

SNS sends email notification



**FOR Reference:**

https://medium.com/@aakashrana7/automating-s3-event-notifications-using-aws-lambda-and-sns-for-real-time-alerts-6379f69b9d11

