---
title: "Testing Presigned S3 Uploads"
weight: 2
chapter: false
pre: " <b> 9.2 </b> "
---

*Validating secure, direct client-to-cloud file transfers.*

A core architectural feature of this system is **Path A (Direct Client-to-S3 Uploads)**. To prevent the EC2 servers from experiencing memory exhaustion when patients upload heavy medical scans, the backend generates temporary Presigned URLs. The browser then uploads files directly to the S3 bucket.

### Step 1: Trigger a File Upload

1. Log in to the application as a patient or doctor.
2. Navigate to the medical records section and upload a test document (e.g., a sample X-ray image or PDF).
3. Open your browser's Developer Tools (F12) and check the **Network** tab. You will see a quick GET request to your API to fetch the token, followed by a PUT request pointing directly to an Amazon S3 URL.

### Step 2: Verify in the S3 Vault

1. Open the Amazon S3 Console and navigate to your private bucket (`smarthealthcare-records-[your-initials]`).
2. Verify that the uploaded file is present in the bucket.

![Developer Tools Network tab showing API call then direct PUT to S3](/images/testing/presigned-s3-upload.png)