---
title: "Asynchronous Messaging (SQS & Notifications)"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

In a healthcare application, tasks like sending appointment confirmation emails or SMS reminders can be slow. If 500 patients book an appointment simultaneously, forcing your web server to wait for an external email provider to respond could exhaust server memory and cause the application to crash.

To solve this, we use an Event-Driven Execution Bus to implement a pattern called **Decoupling**. Your ASP.NET Core API will simply drop a fast, lightweight message into an Amazon SQS queue and immediately return a "Success" response to the patient. A separate AWS Lambda worker will then process that queue at its own pace.

{{% notice tip %}}
**Why not just use background threads in C#?** Using in-memory "fire-and-forget" threads (like `Task.Run()`) is dangerous in the cloud. If your Auto Scaling Group terminates an EC2 instance while a background thread is still waiting to send an email, that notification is permanently lost. Amazon SQS durably stores the data independently from your compute lifecycle.
{{% /notice %}}