---
title: "Configuring Outbound Egress"
weight: 2
chapter: false
pre: " <b> 3.2 </b> "
---

Because the EC2 instances live in Private Subnets, they do not possess public IP addresses and cannot be reached directly from the internet. However, they require outbound internet access to pull Docker container images from Amazon ECR or download OS security updates.

Outbound internet traffic is routed through **NAT Gateways** situated in the Public Subnets.

![NAT Gateway configuration](/images/vpc/nat-gateway.png)