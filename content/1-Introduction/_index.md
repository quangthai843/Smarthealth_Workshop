---
title: "Introduction"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

Welcome to the Smart Healthcare Appointment System workshop. This guide walks you through deploying a production-ready, enterprise-grade application on AWS, transitioning from local development to a highly available cloud architecture.

## Problem Statement

Current healthcare scheduling often relies on fragmented systems, leading to administrative bottlenecks and a disjointed experience for patients. The Smart Healthcare Appointment System is a platform designed to help users find doctors, book appointments, pay online, and manage medical records in a single centralized system.

The project aims to digitize the medical examination process, significantly reduce waiting times, optimize hospital operational workflows, and improve the overall experience for both patients and medical teams. To achieve this, the system must efficiently handle many concurrent booking requests, store sensitive medical records safely, and be deployed following strict multi-layered security principles (Defense in Depth).

## Solution Architecture

To meet these demands, the Smart Healthcare Appointment System is built using a Cloud Native architecture on AWS. It utilizes a Microservice-ready model combined with Clean Architecture to ensure flexible scalability, High Availability (HA), and easy maintenance.

The platform is structured around a highly secure 3-Tier Architecture on AWS:

**Edge & Ingress Tier:** Traffic enters through Amazon CloudFront and AWS WAF for Layer-7 DDoS protection, routing to an Application Load Balancer (ALB) located in the Public Subnets.

**Compute Tier:** The ASP.NET Core API runs inside Docker containers on an Amazon EC2 Auto Scaling Group, completely isolated within Private App Subnets.

**Data Tier:** Amazon RDS PostgreSQL and Amazon ElastiCache (Redis) sit in the deepest Private Data Subnets, strictly accepting traffic only from the compute tier.

**Multi-AZ Resilience:** The architecture spans across multiple Availability Zones (AZs). The RDS PostgreSQL database uses a Multi-AZ deployment (synchronous standby) and the ElastiCache Redis cluster uses a Read Replica to guarantee zero data loss and automated failover in under 60 seconds.

### System Architecture Diagram

![System Architecture Diagram](/images/system-architecture.png)

<!-- TODO: Place the real system diagram at static/images/system-architecture.png (or update the src path above to your file name). -->

## Key Features

**Real-Time Booking & Doctor Management:** Users can instantly find doctors and book time slots. The system uses Amazon ElastiCache (Redis) to cache frequently accessed data (like doctor lists and schedules) to reduce database load and return instant search results.

**Secure File Uploads via Presigned URLs:** When a patient uploads medical records, images, or medical documents, the application generates a secure, temporary Presigned URL. This allows the client browser to upload heavy files directly to Amazon S3, completely removing the file-processing burden from the backend EC2 servers.

**Decoupled Async Notifications:** The platform uses an event-driven execution bus. When an appointment is created, the API instantly writes to the database and drops an event payload into an Amazon SQS FIFO queue. A background AWS Lambda worker processes these messages asynchronously to send Email and SMS reminders via Amazon SES and SNS, ensuring the web API remains blazing fast.

## Technologies Used

The Smart Healthcare Appointment System relies on a modern, cloud-native technology stack:

- **Frontend:** React + TypeScript (bundled via Vite).
- **Backend:** ASP.NET Core 9 (Clean Architecture).
- **Compute:** Docker containers running on Amazon EC2 instances utilizing AWS Graviton (ARM64) processors for up to 40% better price-to-performance.
- **Networking & Edge:** Amazon Route 53 (DNS), Amazon CloudFront (CDN), AWS WAF (Security), AWS Certificate Manager (Free SSL/TLS), Application Load Balancer (ALB), NAT Gateway, and S3 Gateway VPC Endpoints.
- **Database & Storage:** Amazon RDS PostgreSQL (Relational Data), Amazon ElastiCache Redis (In-Memory Cache), and Amazon S3 (Object Storage).
- **Integration & Observability:** Amazon SQS (Message Queuing), AWS Lambda (Async Processing), Amazon SNS/SES (Notifications), and Amazon CloudWatch (Logging & Metrics).