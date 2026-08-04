---
title: "Security Group Topology"
weight: 4
chapter: false
pre: " <b> 3.4 </b> "
---

With network subnets established, we configure virtual firewalls (Security Groups) to enforce our Defense-in-Depth security strategy. Each tier is configured to accept traffic strictly from the tier directly above it.

Navigate to the EC2 Console, click **Security Groups** under Network & Security, and create the following three security groups:

### 1. Load Balancer Security Group (`smarthealthcare-alb-sg`)

Performs initial edge filtering for incoming public user traffic.

- **VPC:** Select `smarthealthcare-vpc`
- **Inbound Rules:**
  - Type: HTTP (Port 80) | Source: Anywhere-IPv4 (0.0.0.0/0)
  - Type: HTTPS (Port 443) | Source: Anywhere-IPv4 (0.0.0.0/0)

![ALB security group inbound rules](/images/vpc/alb-security-group.png)

### 2. Compute Tier Security Group (`smarthealthcare-ec2-sg`)

Protects the ASP.NET Core backend containers. EC2 instances must only accept traffic routed through the ALB.

- **VPC:** Select `smarthealthcare-vpc`
- **Inbound Rules:**
  - Type: Custom TCP | Port Range: 5000 | Source: Custom -> Select `smarthealthcare-alb-sg`

![EC2 security group inbound rules](/images/vpc/ec2-security-group.png)

### 3. Data Tier Security Group (`smarthealthcare-db-sg`)

Isolates PostgreSQL and Redis instances. The data layer strictly accepts queries originating from the compute tier.

- **VPC:** Select `smarthealthcare-vpc`
- **Inbound Rules:**
  - Type: PostgreSQL | Port: 5432 | Source: Custom -> Select `smarthealthcare-ec2-sg`
  - Type: Custom TCP | Port: 6379 | Source: Custom -> Select `smarthealthcare-ec2-sg`

![Database security group inbound rules](/images/vpc/db-security-group.png)

{{% notice warning %}}
**Never Expose Database Ports Publicly:** Never set the source for database ports (5432 or 6379) to 0.0.0.0/0. Referencing the EC2 Security Group ID as the source ensures that database connections are restricted to authorized backend application containers inside the VPC.
{{% /notice %}}