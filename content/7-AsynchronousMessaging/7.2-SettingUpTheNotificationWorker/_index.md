---
title: "Setting up the Notification Worker"
weight: 2
chapter: false
pre: " <b> 7.2 </b> "
---

Now we need a worker to read those messages and execute the actual email/SMS delivery. We will use **AWS Lambda** — a serverless compute engine — so you only pay when an email is actually being sent.

### Step 1: Create the Lambda Function

1. Open the AWS Lambda Console and click **Create function**.
2. Choose **Author from scratch**.
3. Function name: `SmartHealthcareNotificationWorker`.
4. Runtime: Choose your preferred language (e.g., Node.js or Python) since this is a lightweight worker script.
5. Architecture: You can leave this as `x86_64`, or match your EC2 ARM64 setup.
6. Click **Create function**.

### Step 2: Grant Permissions to the Worker

For the Lambda function to do its job, it needs permission to read from your SQS queue and send emails/SMS via Amazon SES and Amazon SNS.

1. In your Lambda function dashboard, navigate to the **Configuration** tab and click **Permissions**.
2. Click the **Role name** under Execution role (this will open the IAM console).
3. Click **Add permissions -> Attach policies**.
4. Search for and attach the following policies:
   - `AmazonSQSFullAccess` (or a custom policy scoped strictly to `sqs:ReceiveMessage` and `sqs:DeleteMessage`).
   - `AmazonSESFullAccess` (To dispatch emails).
   - `AmazonSNSFullAccess` (To dispatch SMS messages).

![Lambda Execution Role with SQS, SES, and SNS policies](/images/sqs-notifications/lambda-permissions.png)

### Step 3: Configure the SQS Trigger

We must tell SQS to automatically wake up the Lambda function whenever a new appointment message arrives.

1. Go back to your Lambda function dashboard.
2. In the Function overview section, click **+ Add trigger**.
3. Trigger configuration: Select **SQS** from the dropdown menu.
4. SQS queue: Select the `smarthealthcare-notifications.fifo` queue you created in Section 7.1.
5. Batch size: Set this to a small number (e.g., **10**) so the Lambda function processes messages in manageable chunks.
6. Click **Add**.

![Lambda designer with SQS trigger connected](/images/sqs-notifications/lambda-sqs-trigger.png)

With this configuration, your core web API is now fully decoupled from external notification services. When an appointment is booked, your C# API will emit a payload to SQS, and the Lambda worker will asynchronously trigger Amazon SES and SNS to handle delivery — ensuring zero performance bottlenecks!