---
title: "Provisioning Amazon ElastiCache Redis"
weight: 2
chapter: false
pre: " <b> 4.2 </b> "
---

To support real-time doctor searches and session management, we need a high-speed caching layer. We will deploy a Redis cluster featuring a Primary node in AZ-A for write operations, and a Read Replica in AZ-B for read operations and automated failover.

### Step 1: Create the ElastiCache Subnet Group

1. Open the ElastiCache Console and click **Subnet groups** on the left menu.
2. Click **Create subnet group**.
3. Name: `smarthealthcare-redis-subnet-group`.
4. VPC ID: Select `smarthealthcare-vpc`.
5. Availability Zones & Subnets: Select both AZs and pick the exact same two private data subnets you used for RDS.
6. Click **Create**.

### Step 2: Create the Redis Cluster

1. On the left menu, click **Redis caches** (or Redis OSS caches), then **Create Redis cluster**.
2. Choose **Design your own cache** and select **Cluster cache**.
3. Cluster mode: Select **Disabled** (We want a simple Primary/Replica topology, not a sharded cluster).
4. Multi-AZ: Ensure this is **Enabled**.
5. Number of Replicas: Set to **1**.
6. Name: `smarthealthcare-redis`.
7. Node type: Choose `cache.t4g.micro`.
8. Under Subnet group, select `smarthealthcare-redis-subnet-group`.
9. Leave encryption in transit enabled and click **Create**.

![Redis cluster configuration](/images/redis/redis-cluster-config.png)

{{% notice note %}}
**Capturing Your Endpoints:** Both RDS and ElastiCache take 5-10 minutes to provision. Once their status changes to Available, click into their details pages and copy down the RDS Endpoint URL, the Redis Primary Endpoint, and the Redis Reader Endpoint. You will need these to connect your backend API.
{{% /notice %}}