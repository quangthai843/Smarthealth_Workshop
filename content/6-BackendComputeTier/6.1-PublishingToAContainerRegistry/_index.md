---
title: "Publishing to a Container Registry"
weight: 1
chapter: false
pre: " <b> 6.1 </b> "
---

Because you designed an enterprise-grade architecture with an Auto Scaling Group, you want **Immutable Infrastructure** — meaning every time a new EC2 server spins up, it automatically pulls your code and starts serving traffic without human intervention.

To do this, we must push your local Docker image to a cloud registry. For this workshop, we will use **Docker Hub** as the easiest public/private registry.

Open your local terminal and log in to Docker Hub:

```bash
docker login
```

Navigate to the folder containing your API code and build the image. Tag it with your Docker Hub username (Replace `yourusername` with your actual username):

```bash
docker build -t yourusername/smarthealthcare-api:latest .
```

Push the finished image up to the cloud:

```bash
docker push yourusername/smarthealthcare-api:latest
```

![Successful docker push to Docker Hub](/images/ec2-alb/docker-push.png)

{{% notice warning %}}
**ARM64 vs x86 Architecture:** We will be using AWS Graviton instances (`t4g.small`) which use ARM64 processors. If you build your Docker image on an older Intel/AMD Windows machine (x86), the container will crash on the Graviton server. Ensure you use Docker buildx (`docker buildx build --platform linux/arm64 ...`) to build an ARM64-compatible image if your local machine is not ARM-based.
{{% /notice %}}