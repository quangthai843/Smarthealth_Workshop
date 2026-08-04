---
title: "Local Verification & Containerization"
weight: 3
chapter: false
pre: " <b> 2.3 </b> "
---

To prepare the ASP.NET Core 9 Clean Architecture backend for AWS EC2 deployment, you must containerize the application using a multi-stage Dockerfile.

### Step 1: Create the Multi-Stage Dockerfile

Place the following Dockerfile inside the root of your `backend/` folder (alongside your SmartHealthcare.sln file):

```dockerfile
# Stage 1: Build & Publish
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /app

# Copy solution and project files for dependency restoration
COPY *.sln ./
COPY src/SmartHealthcare.API/*.csproj ./src/SmartHealthcare.API/
COPY src/SmartHealthcare.Application/*.csproj ./src/SmartHealthcare.Application/
COPY src/SmartHealthcare.Domain/*.csproj ./src/SmartHealthcare.Domain/
COPY src/SmartHealthcare.Infrastructure/*.csproj ./src/SmartHealthcare.Infrastructure/

RUN dotnet restore

# Copy all source code and compile
COPY . ./
RUN dotnet publish src/SmartHealthcare.API/SmartHealthcare.API.csproj -c Release -o /app/out

# Stage 2: Runtime Container
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS runtime
WORKDIR /app

# Copy compiled binaries from build stage
COPY --from=build /app/out .

# Expose HTTP port
EXPOSE 80

# Configure entry point
ENTRYPOINT ["dotnet", "SmartHealthcare.API.dll"]
```

### Step 2: Build the Container Image Locally

Navigate to the `backend/` directory in your terminal and execute the Docker build command:

```bash
cd backend
docker build -t smarthealthcare-api .
```

### Step 3: Spin Up Local Dependencies & Test

Start local PostgreSQL, Redis, and MinIO emulators via Docker Compose:

```bash
docker compose up -d
```

Run your newly built API container:

```bash
docker run -d -p 5000:80 --name smarthealthcare-api-test smarthealthcare-api
```

Test the health endpoint:

```bash
curl http://localhost:5000/health
```

You should receive a plain text response: `Healthy`.

Once verified, stop and remove the test container before proceeding to cloud deployment:

```bash
docker stop smarthealthcare-api-test && docker rm smarthealthcare-api-test
```