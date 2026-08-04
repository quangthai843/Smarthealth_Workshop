---
title: "Configuring the Application Load Balancer"
weight: 3
chapter: false
pre: " <b> 6.3 </b> "
---

Now we create the public entry point for your backend API.

1. In the EC2 Console left menu, click **Load Balancers**.
2. Click **Create load balancer** and choose **Application Load Balancer**.
3. Load balancer name: `smarthealthcare-alb`.
4. Scheme: Select **Internet-facing**.
5. Network mapping:
   - **VPC:** Select `smarthealthcare-vpc`.
   - **Mappings:** You MUST select both of your Availability Zones and choose the **Public Subnets** (e.g., Public Subnet 1A and Public Subnet 1B).
6. Security groups: Deselect the default group and select ONLY `smarthealthcare-alb-sg`.
7. Listeners and routing: For the default **HTTP Port 80** listener, select your `smarthealthcare-tg` Target Group.
8. Scroll to the bottom and click **Create load balancer**.

![ALB Network Mapping with the two Public Subnets](/images/ec2-alb/alb-network-mapping.png)

{{% notice note %}}
**Save Your ALB DNS Name:** Once the ALB says "Active", copy its DNS name (e.g., `smarthealthcare-alb-1234.ap-southeast-1.elb.amazonaws.com`). This is the URL you will paste into your React frontend's `.env.production` file in the next chapter.
{{% /notice %}}