---
title: "Target Group Setup"
weight: 2
chapter: false
pre: " <b> 6.2 </b> "
---

The Target Group tells the Load Balancer exactly which port to send traffic to and how to verify if your application is healthy.

1. Open the EC2 Console, scroll down the left menu, and click **Target Groups**.
2. Click **Create target group**.
3. Target type: Select **Instances**.
4. Target group name: `smarthealthcare-tg`.
5. Protocol & Port: **HTTP** on **Port 5000** (The port your C# API container exposes).
6. VPC: Select `smarthealthcare-vpc`.
7. Health checks: Change the **Health check path** to `/health`.
8. Click **Next**, skip registering instances for now (the Auto Scaling Group will do this automatically later), and click **Create target group**.

![Target Group creation with Port 5000 and /health](/images/ec2-alb/target-group.png)