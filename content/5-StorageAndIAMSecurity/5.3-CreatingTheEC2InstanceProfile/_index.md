---
title: "Creating the EC2 Instance Profile"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

Finally, we attach that policy to an IAM "Role" that we will hand to your EC2 servers. Because your ASP.NET Core code uses the AWS SDK's Default Credential Provider Chain, it will automatically inherit the permissions of this role without needing any hardcoded keys in the `appsettings.json`.

We will also attach a secondary AWS-managed policy so we can securely access the EC2 terminal from our browser later.

1. Still in the IAM Console, click **Roles** on the left menu.
2. Click **Create role**.
3. Trusted entity type: Select **AWS service**.
4. Use case: Select **EC2**, then click **Next**.
5. Add permissions:
   - Search for the policy you just made (`SmartHealthcareS3AccessPolicy`) and check the box next to it.
   - Search for `AmazonSSMManagedInstanceCore` and check the box next to it.
6. Click **Next**.
7. Role name: `SmartHealthcareEC2Role`.
8. Click **Create role**.

![IAM Role creation review](/images/s3-iam/iam-role-review.png)

{{% notice warning %}}
**Private Network Access Requirement:** Because your EC2 server will not have a public IP address, you cannot connect to it using a standard SSH client like PuTTY. Adding the `AmazonSSMManagedInstanceCore` policy is critical because it tells AWS to allow you to open a terminal directly through your browser using AWS Systems Manager (SSM) Session Manager.
{{% /notice %}}