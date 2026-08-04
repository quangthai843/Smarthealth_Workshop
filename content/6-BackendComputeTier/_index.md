---
title: "Backend Compute Tier (EC2 & ALB)"
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

This is the most exciting phase of the deployment. Because your EC2 servers are placed in the Private Subnets — which is exactly what enterprise companies do for security — they cannot be accessed directly from the internet. Instead, public traffic will hit your Application Load Balancer (ALB) in the Public Subnet, and the ALB will securely pass that traffic down to your C# API.

### What You'll Configure

**Container Registry:** Pushing your locally built ASP.NET Core Docker image to a cloud registry (like Docker Hub or Amazon ECR) so your servers can download it.

**Target Group:** Defining the internal routing rules so the load balancer knows to send traffic to Port 5000 and monitor the `/health` endpoint.

**Application Load Balancer (ALB):** Spanning the public gateway across multiple Availability Zones to receive internet traffic.

**EC2 Auto Scaling Group (ASG):** Creating a Launch Template with AWS Graviton (ARM64) instances and a User Data script for automated, zero-touch container deployment.