---
title: "Creating the Secure S3 Bucket"
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

Open the S3 Console and click **Create bucket**.

- **Bucket name:** Enter a globally unique name (e.g., `smarthealthcare-records-[your-initials]`).
- Write this exact name down. You will need to put it in your EC2 environment variables later so your ASP.NET Core API knows where to upload files.
- **AWS Region:** Select your region (`ap-southeast-1` to match your VPC).

**Block Public Access settings for this bucket:**

- Ensure **Block all public access** is CHECKED. This ensures no one on the internet can view a medical record URL without going through your authenticated C# API and obtaining a temporary Presigned URL.

**Bucket Versioning:**

- Leave as **Disable** (unless you want to keep track of older versions of edited files, though this increases storage costs).

**Default encryption:**

- Leave as **Server-side encryption with Amazon S3 managed keys (SSE-S3)**.

Click **Create bucket**.

![S3 bucket creation with Block Public Access and SSE-S3](/images/s3-iam/s3-bucket-create.png)