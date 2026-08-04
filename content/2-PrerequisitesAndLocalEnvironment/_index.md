---
title: "Prerequisites"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Before deploying the Smart Healthcare Appointment System to AWS, you must set up your local development environment. This ensures you can build, test, and containerize the application locally prior to cloud deployment.

Ensure the following tools are installed on your workstation:

- **Node.js (v18 or higher) & npm/pnpm:** Required for building and running the React (Vite) frontend.
- **.NET 9 SDK:** Required to compile and run the ASP.NET Core 9 backend solution locally.
- **Docker Desktop:** Required to build container images and run local dependency containers (PostgreSQL, Redis, MinIO).
- **AWS CLI (v2):** Required to interact with AWS services, manage profiles, and deploy resources from your terminal.
- **Git:** For source control and cloning the repository.

### Verify Installation

Open your terminal or PowerShell and run the following verification commands:

```bash
# Check Node.js version (Should be 18+)
node --version

# Check .NET SDK version (Should be 9.0.x)
dotnet --version

# Check Docker version and status
docker --version

# Check AWS CLI version
aws --version

# Check Git version
git --version
```