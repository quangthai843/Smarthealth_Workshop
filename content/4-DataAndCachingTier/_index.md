---
title: "Data & Caching Tier (RDS & Redis)"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

The persistence layer of the Smart Healthcare Appointment System relies on highly available, managed database services. To ensure patient data is never lost during a data center outage, we will provision our databases across multiple Availability Zones (Multi-AZ) within our strictly isolated Private Data Subnets.

### What You'll Configure

**Database Subnet Groups:** Explicitly defining network boundaries to ensure your data tier launches exclusively within the isolated private subnets, completely hidden from the public internet.

**Amazon RDS PostgreSQL:** Provisioning the core relational database using a Multi-AZ deployment (hot standby) to guarantee automated failover and zero data loss.

**Amazon ElastiCache Redis:** Deploying a high-speed caching cluster featuring a Primary write node and an asynchronous Read Replica to accelerate doctor searches and optimize API performance.