---
title: "Creating the S3 Gateway VPC Endpoint"
weight: 3
chapter: false
pre: " <b> 3.3 </b> "
---

When private EC2 instances interact with Amazon S3 (such as issuing S3 Presigned URLs or checking medical document metadata), sending that internal traffic through the NAT Gateway incurs additional data processing charges.

To optimize cost and performance, we configure an **Amazon S3 Gateway VPC Endpoint**:

1. Locate the **VPC endpoints** dropdown within the VPC Wizard.
2. Select **S3 Gateway**.
3. Click **Create VPC** at the bottom of the page.

![VPC creation success checklist](/images/vpc/vpc-creation-success.png)