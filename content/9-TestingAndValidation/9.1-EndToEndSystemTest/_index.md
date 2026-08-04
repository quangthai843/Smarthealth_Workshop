---
title: "End-to-End System Test"
weight: 1
chapter: false
pre: " <b> 9.1 </b> "
---

*Verifying API health, database connections, and frontend routing.*

Before simulating failures, we must confirm that the steady-state architecture is routing traffic correctly across all AWS layers.

### Step 1: Verify API & ALB Health

1. Open the EC2 Console and navigate to **Target Groups**.
2. Select `smarthealthcare-tg` and check the **Targets** tab.
3. You should see your EC2 instance(s) listed with a **Healthy** status. This confirms the Application Load Balancer (ALB) is successfully reaching the `/health` endpoint on your Dockerized ASP.NET Core container.

![Target Group Targets tab with Healthy instances](/images/testing/target-group-healthy.png)

### Step 2: Test Frontend & Database Seeding

1. Open your browser and navigate to your CloudFront Distribution domain (e.g., `https://d12345abcdef.cloudfront.net`).
2. The React frontend should load instantly.
3. Attempt to log in using the default **Admin** or **Doctor** credentials specified in your repository's README.md.

A successful login proves two things:

- **Frontend Routing:** CloudFront is correctly forwarding API calls to the ALB.
- **Database Seeding:** Entity Framework Core successfully ran the `MigrateAsync` command to build tables and insert the seed data into your private RDS PostgreSQL database on the first boot.