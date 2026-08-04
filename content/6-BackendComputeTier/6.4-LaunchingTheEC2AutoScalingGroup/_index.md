---
title: "Launching the EC2 Auto Scaling Group"
weight: 4
chapter: false
pre: " <b> 6.4 </b> "
---

An Auto Scaling Group needs instructions on how to build a new server. You provide this by creating an EC2 Launch Template.

### Step 1: Create the Launch Template

1. In the EC2 Console left menu, click **Launch Templates**, then **Create launch template**.
2. Name: `smarthealthcare-api-template`.
3. AMI: Select **Amazon Linux 2023** (Make sure to select the **64-bit (Arm)** architecture version).
4. Instance type: Select **t4g.small** (AWS Graviton).
5. Key pair: Select **Proceed without a key pair** (We are using IAM/SSM to connect, so SSH keys are unnecessary).
6. Network settings:
   - Do not specify a subnet here (the ASG will handle it).
   - Security groups: Select `smarthealthcare-ec2-sg`.
7. Advanced details:
   - IAM instance profile: Select `SmartHealthcareEC2Role`.
8. Scroll to the very bottom to **User data** and paste the following bootstrap script (Replace the placeholder variables with your actual RDS, Redis, and S3 details):

```bash
#!/bin/bash
# 1. Update OS and install Docker
dnf update -y
dnf install -y docker
systemctl start docker
systemctl enable docker
usermod -a -G docker ec2-user

# 2. Pull your web app image from Docker Hub
docker pull yourusername/smarthealthcare-api:latest

# 3. Run the container with all production Environment Variables
docker run -d \
  -p 5000:80 \
  --name api \
  -e "ConnectionStrings__DefaultConnection=Host=YOUR_RDS_ENDPOINT;Database=smarthealthcare;Username=postgres;Password=YOUR_RDS_PASSWORD" \
  -e "ConnectionStrings__Redis=YOUR_REDIS_PRIMARY_ENDPOINT:6379,ssl=true" \
  -e "ConnectionStrings__RedisReader=YOUR_REDIS_READER_ENDPOINT:6379,ssl=true" \
  -e "Aws__S3__BucketName=YOUR_S3_BUCKET_NAME" \
  -e "Aws__UseLocalEmulators=false" \
  yourusername/smarthealthcare-api:latest
```

9. Click **Create launch template**.

![Launch Template Advanced Details with IAM profile and User Data](/images/ec2-alb/launch-template.png)

### Step 2: Create the Auto Scaling Group (ASG)

1. In the EC2 Console left menu, click **Auto Scaling Groups**, then **Create Auto Scaling group**.
2. Name: `smarthealthcare-asg`.
3. Launch template: Select `smarthealthcare-api-template` and click **Next**.
4. Network: Select `smarthealthcare-vpc` and choose both of your **Private App Subnets**.
5. Load balancing: Select **Attach to an existing load balancer** and choose your target group (`smarthealthcare-tg`).
6. Health checks: Turn on **Turn on Elastic Load Balancing health checks**.
7. Group size: Set **Desired capacity** to 1, **Minimum** to 1, and **Maximum** to 2 (for demo purposes).
8. Skip through the remaining steps and click **Create Auto Scaling group**.

Within a few minutes, your ASG will launch an EC2 instance, install Docker, pull your image, and connect to RDS and Redis. The ALB will ping the `/health` endpoint, and once it receives a 200 OK, your backend API is officially live on the internet.