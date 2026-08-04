---
title: "Injecting Environment Variables"
weight: 1
chapter: false
pre: " <b> 8.1 </b> "
---

Because Vite compiles your application into static files, it cannot read environment variables from a server at runtime. We must "bake" the API URL into the code during the build process.

### Step 1: Clone the Frontend Repository

First, you need to pull the React frontend codebase to your local machine. Open your terminal and run:

```bash
git clone https://github.com/quangthai843/Smarthealth.git
cd Smarthealth/frontend
```

(Note: If the React code sits at the root of the repository, just run `cd Smarthealth` instead).

### Step 2: Create the Production Environment File

In the root of your frontend folder (next to package.json), create a file named exactly `.env.production`. Paste your Application Load Balancer (ALB) DNS name into the file, ensuring you use the `VITE_` prefix required by Vite:

```env
VITE_API_URL=http://smarthealthcare-alb-1234.ap-southeast-1.elb.amazonaws.com
```

{{% notice note %}}
**Centralized API Client:** Your React codebase should have a centralized Axios or Fetch client that references `import.meta.env.VITE_API_URL`. When you build the app for production, Vite will automatically swap out the localhost URL for your live AWS Load Balancer URL.
{{% /notice %}}