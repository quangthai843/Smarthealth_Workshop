---
title: "Creating the SQS FIFO Queue"
weight: 1
chapter: false
pre: " <b> 7.1 </b> "
---

We will create an Amazon SQS FIFO (First-In-First-Out) queue. FIFO queues ensure that messages are processed in the exact order they were sent and exactly once, preventing patients from receiving duplicate appointment confirmations.

1. Open the Amazon SQS Console and click **Create queue**.
2. Type: Select **FIFO**.
3. Name: `smarthealthcare-notifications.fifo` (Note: SQS requires FIFO queue names to strictly end with the `.fifo` suffix).
4. Configuration: Leave the default settings (Visibility timeout, Message retention period, etc.) as they are for now.
5. Access policy: Leave as **Basic** and ensure the queue owner has full permissions.
6. Click **Create queue**.

![SQS Queue creation with FIFO type](/images/sqs-notifications/sqs-queue-create.png)

{{% notice note %}}
**EC2 Permissions:** Remember that your EC2 instance needs permission to send messages to this queue. In a production environment, you would update your `SmartHealthcareEC2Role` (created in Phase 2) to include the `sqs:SendMessage` permission for this specific queue's ARN.
{{% /notice %}}