---
title: "Architecture Cost Breakdown"
weight: 2
chapter: false
pre: " <b> 10.2 </b> "
---

Understanding cloud economics is a core competency for an AWS Solutions Architect. By deliberately choosing specific configurations, we balanced the Cost-Conscious MVP requirements against an Enterprise Scale-Up roadmap.

Below is the comparative monthly pricing analysis for the Smart Healthcare architecture:

| Architectural Area | MVP / University Demo | Enterprise Production | Cost Optimization Justification |
|---|---|---|---|
| Compute Engine | 1x t4g.small (AWS Graviton) | Amazon ECS Fargate or Multi-AZ m6g.large EC2 | Graviton (ARM64) processors provide up to 20% cost savings and 40% faster performance for ASP.NET Core 9 compared to standard x86 instances. |
| Outbound Routing | 1x NAT Gateway (~$35/mo) | 2x NAT Gateways (~$70/mo) | Using a single NAT Gateway cuts fixed egress costs in half during the demo phase, while maintaining private subnet isolation. |
| Database HA | Single-AZ PostgreSQL db.t4g.micro | Multi-AZ RDS Deployment | Single-AZ cuts the database cost in half. Multi-AZ doubles the cost by running a synchronous hot standby in a second data center. |
| File Storage | Amazon S3 via Presigned URLs | Amazon S3 via Presigned URLs | Using Presigned URLs allows direct browser-to-S3 uploads, keeping heavy medical files off the EC2 instances and allowing the use of cheaper compute sizing. |
| Internal Data | Free S3 Gateway VPC Endpoint | Free S3 Gateway VPC Endpoint | Routing EC2-to-S3 traffic through the Gateway VPC Endpoint eliminates NAT data transfer fees entirely. |