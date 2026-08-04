---
title: "Provisioning Amazon RDS PostgreSQL"
weight: 1
chapter: false
pre: " <b> 4.1 </b> "
---

Before launching the database, we must explicitly tell AWS which private subnets are allowed to host our database instances by creating a DB Subnet Group.

### Step 1: Create the RDS DB Subnet Group

1. Open the RDS Console and click **Subnet groups** on the left menu.
2. Click **Create DB subnet group**.
3. Name: `smarthealthcare-db-subnet-group`.
4. VPC: Select your `smarthealthcare-vpc`.
5. Add subnets: Select both of your Availability Zones. Under the subnet dropdown, ensure you select the two private subnets designated for your data tier.
6. Click **Create**.

![DB Subnet Group creation](/images/rds/db-subnet-group.png)

### Step 2: Create the RDS Database

1. In the RDS Console, click **Create database**.
2. Choose **Standard create** and select **PostgreSQL** as the engine type.
3. Under Templates, select **Dev/Test**.
4. Under Availability and durability, select **Multi-AZ DB Instance** (This instructs AWS to automatically create a synchronous hot standby replica in your second AZ).

**Credentials:**

- DB instance identifier: `smarthealthcare-db`.
- Master username: `postgres`.
- Master password: Create a highly secure password. Save this password, as we will inject it into the EC2 environment variables later.

**Instance configuration:**

- Select **Burstable classes** and choose `db.t4g.micro`. This uses AWS Graviton (ARM64) processors to deliver better performance at a lower cost.

**Connectivity:**

- Virtual private cloud (VPC): Select `smarthealthcare-vpc`.
- DB Subnet Group: Select `smarthealthcare-db-subnet-group`.
- Public access: No.
- VPC security group: Choose **Select existing** and assign `smarthealthcare-db-sg`.

5. Click **Create database**.

{{% notice warning %}}
**Cost Optimization for Workshops:** Multi-AZ deployments immediately double your compute and storage costs because AWS provisions a duplicate standby server. If you are building this for a low-cost academic demo, you can select the Free tier template or choose Single DB instance instead. You can easily toggle Multi-AZ back on later before deploying to production.
{{% /notice %}}