---
title: "S3 Static Website Hosting"
weight: 3
chapter: false
pre: " <b> 8.3 </b> "
---

We will create an S3 bucket to act as our web server.

### Step 1: Create the Frontend Bucket

1. Log in to the AWS Console, navigate to S3, and click **Create bucket**.
2. Bucket name: Enter a globally unique name (e.g., `smarthealthcare-frontend-[your-initials]`).
3. AWS Region: Select `ap-southeast-1` to match your backend.

### Step 2: Configure Public Access

Unlike your private medical records bucket, this bucket must be readable by anyone on the internet.

1. **Uncheck** Block all public access and acknowledge the warning.
2. After creating the bucket, go to the **Permissions** tab.
3. Scroll down to **Bucket policy**, click **Edit**, and paste this JSON (replace `YOUR_BUCKET_NAME` with your actual bucket name):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
        }
    ]
}
```

![S3 Bucket Policy editor with public read policy](/images/frontend/s3-bucket-policy.png)

### Step 3: Enable Static Hosting & Upload

1. Go to the **Properties** tab, scroll to the bottom, and click **Edit** under Static website hosting.
2. Select **Enable**.
3. Set both the **Index document** and **Error document** to `index.html`. Save your changes.
4. Go to the **Objects** tab and click **Upload**.
5. Drag and drop the contents of your `dist` folder into AWS (do not upload the `dist` folder itself — `index.html` must sit at the very root of the bucket). Click **Upload**.