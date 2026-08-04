---
title: "Creating the 3-Tier VPC"
weight: 1
chapter: false
pre: " <b> 3.1 </b> "
---

Before opening the AWS console, let's inspect how the 10.0.0.0/16 IP space is allocated across 3 distinct tiers and 2 Availability Zones:

- **Public Subnets (10.0.1.0/24 & 10.0.2.0/24):** Host public-facing ingress components, specifically the Application Load Balancer (ALB) and NAT Gateways.
- **Private App Subnets (10.0.11.0/24 & 10.0.12.0/24):** Host the compute layer (EC2 Auto Scaling Group running ASP.NET Core API containers).
- **Private Data Subnets (10.0.21.0/24 & 10.0.22.0/24):** Host the persistence tier (Amazon RDS PostgreSQL and Amazon ElastiCache Redis).

![3-Tier VPC Network Diagram](/images/vpc/3-tier-vpc.png)

{{% notice note %}}
**Structural Isolation:** Splitting subnets into dedicated functional tiers ensures that database instances cannot accidentally receive public IP addresses or route directly to internet gateways.
{{% /notice %}}

### Step 1: Launch the VPC Wizard

Open the AWS Management Console and search for **VPC**.

1. On the VPC Dashboard, click the orange **Create VPC** button.
2. Select the **VPC and more** option to reveal the visual topology builder.

### Step 2: Configure the Network Topography

Apply the following configurations to match the enterprise architecture blueprint:

- **Name tag auto-generation:** `smarthealthcare`.
- **IPv4 CIDR block:** `10.0.0.0/16`.
- **Number of Availability Zones (AZs):** Select **2** (Ensures high availability across two physically separate data centers).
- **Number of public subnets:** Select **2**.
- **Number of private subnets:** Select **4** (AWS will allocate 2 for the EC2 App tier and 2 for the Data tier).

![VPC and more creation screen](/images/vpc/vpc-wizard-config.png)