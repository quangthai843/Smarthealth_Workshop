---
title: "Full Environment Teardown"
weight: 1
chapter: false
pre: " <b> 10.1 </b> "
---

When your project or workshop is completely finished, you must destroy all resources to prevent any runaway recurring charges. Follow this exact sequence, as certain resources cannot be deleted if others still depend on them.

### Step 1: Destroy Compute & Ingress

- **Auto Scaling Group:** Go to the EC2 Console -> Auto Scaling Groups, select `smarthealthcare-asg`, and click **Delete**.
- **Application Load Balancer:** Navigate to Load Balancers, select `smarthealthcare-alb`, and click **Delete**.
- **Target Groups:** Navigate to Target Groups and delete `smarthealthcare-tg`.

### Step 2: Destroy Data & Messaging

- **RDS:** In the RDS Console, select your database and click **Delete** (Uncheck "Create final snapshot" to avoid storage fees).
- **ElastiCache:** In the ElastiCache Console, select your Redis cluster and click **Delete**.
- **SQS:** Navigate to the SQS Console, select `smarthealthcare-notifications.fifo`, and click **Delete**.

### Step 3: Destroy Storage & CDN

- **CloudFront:** Disable your CloudFront distribution first (this takes a few minutes), then click **Delete**.
- **S3 Buckets:** Navigate to S3. You must empty the buckets first before AWS allows you to delete them. Select your frontend and records buckets, click **Empty**, confirm the deletion of all objects, and then click **Delete** on the buckets themselves.

### Step 4: Destroy Network Infrastructure

- **NAT Gateways:** Go to the VPC Console -> NAT Gateways, and delete your gateways.
- **Elastic IPs:** Navigate to Elastic IPs on the left menu, select the unattached IPs, click **Actions**, and select **Release Elastic IP addresses** (AWS charges for IPs that are held but not attached to running resources).
- **VPC:** Finally, navigate to Your VPCs, select `smarthealthcare-vpc`, and click **Delete VPC**. This will automatically clean up your subnets, route tables, and security groups.

![Release Elastic IP addresses confirmation screen](/images/cleanup/release-elastic-ip.png)