---
title: "Defining the IAM Access Policy"
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

Now we need to define exactly what your backend is allowed to do. We follow the principle of "least privilege" — your API should only be able to interact with this specific bucket, and nothing else in your AWS account.

1. Open the IAM Console (Identity and Access Management).
2. On the left menu, click **Policies**, then click the **Create policy** button.
3. In the Policy editor, click the **JSON** button on the top right.
4. Delete whatever is in the box, and paste this exact JSON (Replace `YOUR_BUCKET_NAME` with the actual name of the S3 bucket you just created):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::YOUR_BUCKET_NAME",
                "arn:aws:s3:::YOUR_BUCKET_NAME/*"
            ]
        }
    ]
}
```

5. Click **Next** at the bottom.
6. Policy name: `SmartHealthcareS3AccessPolicy`.
7. Click **Create policy**.

![IAM Policy JSON editor](/images/s3-iam/iam-policy-json.png)