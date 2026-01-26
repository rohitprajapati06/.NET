# AZ-204 – Implement Containerized Solutions

> **Purpose**: These notes are written specifically for **Azure Developer Associate (AZ-204)** exam preparation.
> You can directly push this file to GitHub as a `.md` file.

---

## 1. Implement Containerized Solutions – Overview

Containerization is a key skill tested in AZ-204. Microsoft expects developers to understand how to:

* Build and manage container images
* Store images securely in Azure
* Run containers without managing servers
* Deploy modern, scalable container-based applications

Azure focuses on **Docker + Azure-native container services**.

---

## 2. Create and Manage Container Images for Solutions

### What Is a Container Image?

A **container image** is an immutable package that contains:

* Application code
* Runtime
* Libraries and dependencies
* Configuration

Images are built once and run consistently across environments.

---

### Docker Fundamentals (EXAM FAVORITE)

Key Docker components:

| Component  | Description                    |
| ---------- | ------------------------------ |
| Dockerfile | Instructions to build an image |
| Image      | Read-only template             |
| Container  | Running instance of an image   |

---

### Sample Dockerfile (.NET Example)

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY . .
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

---

### Important Docker Commands (Must Remember)

```bash
# Build image
docker build -t myapp .

# List images
docker images

# Run container
docker run -d -p 8080:80 myapp

# List running containers
docker ps

# Stop container
docker stop <container-id>
```

---

## 3. Publish an Image to Azure Container Registry (ACR)

### What Is Azure Container Registry?

Azure Container Registry (ACR) is a **private Docker registry** used to store and manage container images securely.

### Why Use ACR?

* Azure Active Directory authentication
* Private image storage
* Integration with ACI, AKS, Container Apps, App Service
* Supports geo-replication (Premium tier)

---

### Create Azure Container Registry

```bash
az acr create \
  --resource-group myRG \
  --name myacr \
  --sku Basic
```

---

### Push Image to ACR

```bash
# Login to ACR
az acr login --name myacr

# Tag image
docker tag myapp myacr.azurecr.io/myapp:v1

# Push image
docker push myacr.azurecr.io/myapp:v1
```

---

### ACR Authentication (EXAM POINT)

* Azure AD-based authentication
* Managed Identity support
* No credentials stored in code

---

## 4. Run Containers by Using Azure Container Instances (ACI)

### What Is Azure Container Instances?

Azure Container Instances (ACI) is a **serverless container runtime** that allows you to run containers **without managing VMs or orchestrators**.

---

### When to Use ACI?

* Simple container workloads
* Background jobs
* Batch processing
* Event-driven containers

---

### Run Container Using ACI

```bash
az container create \
  --resource-group myRG \
  --name mycontainer \
  --image myacr.azurecr.io/myapp:v1 \
  --cpu 1 \
  --memory 1.5 \
  --ports 80
```

---

### ACI Key Characteristics

| Feature       | Details          |
| ------------- | ---------------- |
| Serverless    | No VM management |
| Billing       | Per second       |
| Scaling       | Manual           |
| Orchestration | ❌ Not supported  |

---

## 5. Create Solutions by Using Azure Container Apps

### What Are Azure Container Apps?

Azure Container Apps is a **managed container platform** built on Kubernetes, but without requiring Kubernetes management.

It combines:

* Kubernetes
* Serverless scaling
* Microservices-friendly architecture

---

### Key Features of Container Apps

* Automatic scaling (KEDA-based)
* HTTP ingress
* Revision management
* Dapr integration
* Managed environment

---

### When to Use Container Apps? (EXAM FAVORITE)

Use Azure Container Apps when:

* You want Kubernetes power without Kubernetes complexity
* You need auto-scaling based on events or HTTP traffic
* You are building microservices

---

### Create Container App Environment

```bash
az containerapp env create \
  --name my-env \
  --resource-group myRG \
  --location eastus
```

---

### Deploy Container App

```bash
az containerapp create \
  --name myapp \
  --resource-group myRG \
  --environment my-env \
  --image myacr.azurecr.io/myapp:v1 \
  --target-port 80 \
  --ingress external
```

---

## 6. ACI vs Container Apps vs AKS (Exam Comparison)

| Feature       | ACI | Container Apps | AKS  |
| ------------- | --- | -------------- | ---- |
| Serverless    | ✅   | ✅              | ❌    |
| Auto Scaling  | ❌   | ✅              | ✅    |
| Kubernetes    | ❌   | Abstracted     | Full |
| Microservices | ❌   | ✅              | ✅    |
| Complexity    | Low | Medium         | High |

---

## 7. AZ-204 Exam Tips (VERY IMPORTANT)

* **ACR** → Private container registry
* **ACI** → Simple, serverless containers
* **Container Apps** → Microservices, auto-scale, Kubernetes without managing it
* **AKS** → Full control, full Kubernetes

### Most Asked Keywords

* Dockerfile
* Image vs container
* ACR authentication
* Serverless containers
* Auto-scaling

---

## 8. Summary

For AZ-204, focus on **why** you choose a service:

* Simple job → **ACI**
* Microservices + scaling → **Container Apps**
* Full Kubernetes control → **AKS**
* Image storage → **ACR**

---

> **Next Steps**
>
> * Practice Azure CLI commands
> * Revise comparison tables
> * Expect scenario-based questions in the exam
