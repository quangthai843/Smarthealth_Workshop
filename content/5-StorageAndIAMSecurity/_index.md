---
title: "Storage & IAM Security"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

Phase 2 of the deployment process focuses on securing the application's sensitive data. Because you are building a healthcare system, security is paramount. Medical records (like diagnoses, prescriptions, and scans) cannot be exposed to the public web.

We will provision an Amazon S3 bucket that is strictly private, and then use AWS Identity and Access Management (IAM) to grant your backend seamless, passwordless access to it.

### What You'll Configure

**Secure Object Storage:** Creating an Amazon S3 bucket with "Block Public Access" enabled and default encryption at rest for storing medical assets.

**Custom IAM Policy:** Writing a JSON permission policy following the principle of least privilege, ensuring the API can only read and write to this specific bucket.

**EC2 Instance Profile:** Creating an IAM Role that attaches the security policy to your compute instances, eliminating the need to hardcode sensitive Access Keys in your application code.