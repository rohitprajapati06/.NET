# Azure App Service – Complete AZ-204 Notes

> **Purpose**: This document is written for **Azure Developer Associate (AZ-204)** exam preparation.
> It is designed to be pushed directly into GitHub as a `.md` file.

---

## 1. What is Azure App Service?

Azure App Service is a **fully managed Platform as a Service (PaaS)** that allows developers to build, deploy, host, and scale **web applications and APIs** without managing servers, operating systems, or runtime infrastructure.

### Key Characteristics

* No server or VM management
* Built-in load balancing and high availability
* Supports multiple programming languages
* Integrated CI/CD
* Enterprise-grade security

---

## 2. Types of Applications You Can Host

### 2.1 Web Apps

* Traditional web applications
* MVC, Razor Pages, SPA backends

### 2.2 Web API Apps

* REST APIs
* Backend services for mobile and web apps

### 2.3 Mobile App Backends

* Authentication
* Push notifications
* Offline sync (legacy but still examinable)

### 2.4 WebJobs

* Background processing
* Continuous or triggered jobs

---

## 3. Supported Languages and Runtimes

### Built-in Runtimes

* .NET / ASP.NET Core
* Java
* Node.js
* Python
* PHP
* Ruby

### Custom Containers

* Docker containers (Linux App Service only)
* Used when custom OS libraries or runtime versions are required

```
AZ-204 Focus:
Know when to choose built-in runtime vs container-based deployment
```

---

## 4. App Service Plan (CRITICAL FOR EXAM)

An **App Service Plan** defines the **compute resources** used by one or more App Services.

### What an App Service Plan Controls

* CPU
* Memory (RAM)
* Operating System (Windows / Linux)
* Pricing tier
* Scaling capabilities

> Multiple apps in the same plan **share the same resources**.

---

## 5. App Service Plan Pricing Tiers

| Tier             | Use Case                  |
| ---------------- | ------------------------- |
| Free (F1)        | Learning & testing        |
| Shared (D1)      | Low traffic apps          |
| Basic (B1–B3)    | Dev/Test                  |
| Standard (S1–S3) | Production                |
| Premium (P1–P3)  | High scale                |
| Isolated (ASE)   | Enterprise, VNet isolated |

```
Exam Rule:
Backups, autoscale, deployment slots require Standard tier or higher
```

---

## 6. Scaling in Azure App Service

### 6.1 Scale Up (Vertical Scaling)

* Change App Service Plan tier
* More CPU and RAM
* Causes application restart

### 6.2 Scale Out (Horizontal Scaling)

* Increase number of instances
* No code changes
* Supports autoscaling

### Autoscale Triggers

* CPU percentage
* Memory usage
* HTTP queue length
* Schedule-based rules

---

## 7. Deployment Methods

### Supported Deployment Options

* ZIP Deploy
* GitHub Actions (CI/CD)
* Azure DevOps Pipelines
* FTP / FTPS
* Local Git
* Docker Container Registry

```
AZ-204 Tip:
GitHub Actions and Azure DevOps are the most tested
```

---

## 8. Deployment Slots (VERY IMPORTANT)

Deployment slots allow you to deploy new versions **without downtime**.

### Common Slots

* Production
* Staging
* Testing

### Slot Swap

* Warm-up before swap
* Zero-downtime release
* Rollback supported

### Slot Settings

* App settings and connection strings can be marked as **slot-specific**

---

## 9. Configuration and App Settings

### Application Settings

* Stored securely
* Exposed as environment variables
* Can be slot-specific

### Connection Strings

* Encrypted at rest
* Automatically injected into runtime

### Startup Configuration

* `web.config` (Windows)
* Startup command (Linux)

---

10. Security Features (AZ-204 EXAM CRITICAL)

Security is a high‑weight area in AZ‑204. You must clearly understand Authentication & Authorization (Easy Auth) and Managed Identity, how they work, and when to use each.

10.1 Authentication & Authorization (Easy Auth)

Azure App Service provides built‑in authentication and authorization, commonly called Easy Auth. It allows you to secure your application without writing authentication code.

What Easy Auth Does

Handles user sign‑in

Validates tokens

Manages identity sessions

Injects identity information into requests

Your application focuses on business logic, while App Service handles authentication.

Supported Identity Providers

Easy Auth supports the following providers (commonly tested in AZ‑204):

Azure Active Directory (Azure AD) – enterprise identity provider

Microsoft Account – consumer Microsoft identities

Google – OAuth 2.0 based authentication

Facebook – OAuth based authentication

Authentication Flow (Conceptual)

User accesses the App Service URL

App Service redirects user to the identity provider

Provider authenticates the user

Provider issues a token (JWT)

App Service validates the token

Request is forwarded to the app with identity headers

Authorization Options

Easy Auth supports multiple authorization behaviors:

Allow anonymous requests (optional authentication)

Require authentication for all requests

Restrict access to specific users or Azure AD groups

Identity Information in the App

After authentication, identity data is available via:

HTTP headers (e.g., X-MS-CLIENT-PRINCIPAL)

Environment variables

Platform SDKs

This allows role‑based logic inside the application.

AZ‑204 Exam Notes – Easy Auth

No code required for authentication

Works at the platform level, before your app executes

Common scenario: “Secure an App Service quickly using Azure AD”

10.2 Managed Identity (EXAM FAVORITE)

Managed Identity is used for service‑to‑service authentication, not user authentication.

It allows an App Service to securely access Azure resources without storing secrets, keys, or passwords in code.

Types of Managed Identity
1. System‑Assigned Managed Identity

Enabled directly on the App Service

Identity lifecycle is tied to the App Service

Deleted automatically when the app is deleted

2. User‑Assigned Managed Identity

Created as a standalone Azure resource

Can be shared across multiple services

Lifecycle managed independently

How Managed Identity Works

Azure creates an identity in Azure AD

App Service obtains a token from Azure AD at runtime

Token is used to access Azure services

No credentials are stored in code or configuration

Secure Access to Azure Resources

Managed Identity is commonly used to access:

Azure SQL Database – using Azure AD authentication

Azure Key Vault – retrieve secrets, keys, certificates

Azure Storage Accounts – Blob, Queue, Table access

Access is granted using Azure RBAC or Key Vault access policies.

Example – Managed Identity with Key Vault (Conceptual)

App Service has Managed Identity enabled

Identity is granted Get permission on Key Vault secrets

App retrieves secret at runtime using Azure SDK

No connection strings or secrets stored in code

Why “No Secrets in Code” Is Critical

Without Managed Identity:

Secrets stored in appsettings.json

Keys exposed in pipelines or repositories

Manual rotation required

With Managed Identity:

Secrets are never stored

Automatic token rotation

Strong security posture (exam favorite)

10.3 Easy Auth vs Managed Identity (EXAM COMPARISON)
Feature	Easy Auth	Managed Identity
Purpose	User authentication	Service authentication
Used for	Login & access control	Access Azure resources
Works with	Users	Azure services
Stores secrets	No	No
Exam usage	Secure app endpoints	Secure backend access
10.4 AZ‑204 Common Security Scenarios

Secure web app login → Easy Auth + Azure AD

Access Key Vault securely → Managed Identity

Remove secrets from config → Managed Identity

Authenticate users quickly without code → Easy Auth

## 11. Networking Capabilities

### VNet Integration

* Access private resources
* Outbound traffic to VNet

### Private Endpoint

* Inbound private access
* No public exposure

### IP Restrictions

* Allow/Deny traffic by IP

### Hybrid Connections

* Access on-premises systems

---

## 12. Monitoring and Diagnostics

### Application Insights

* Request tracking
* Dependency tracking
* Live metrics
* Distributed tracing

### Logs

* Application logs
* Web server logs
* Failed request tracing

---

## 13. Backup and Restore

* Available from **Standard tier and above**
* Automatic scheduled backups
* Stored in Azure Storage
* Restore full app or configuration only

---

## 14. App Service vs Azure Functions

| Feature  | App Service     | Azure Functions  |
| -------- | --------------- | ---------------- |
| Model    | Always running  | Event-driven     |
| Billing  | Fixed plan      | Consumption      |
| Use Case | Web apps & APIs | Serverless logic |

---

## 15. High Availability

Azure App Service provides high availability by:

* Running multiple instances
* Load balancing traffic automatically
* Automatic OS patching

---

## 16. Common AZ-204 Exam Scenarios

* Zero downtime deployment → Deployment Slots
* Increase CPU/RAM → Scale Up
* Handle traffic spikes → Scale Out
* Secure resource access → Managed Identity
* Monitor failures → Application Insights

---

## 17. Final Exam Summary

Azure App Service is a **core AZ-204 topic**.

You must be confident with:

* App Service Plans
* Scaling strategies
* Deployment slots
* CI/CD integration
* Security & Managed Identity
* Monitoring and diagnostics

---

## 18. Azure App Service – Azure CLI & PowerShell Commands (EXAM CRITICAL)

### 18.1 Login

```bash
az login
```

---

### 18.2 Create Resource Group

```bash
az group create \
  --name rg-az204-appservice \
  --location eastus
```

---

### 18.3 Create App Service Plan

```bash
az appservice plan create \
  --name asp-az204-linux \
  --resource-group rg-az204-appservice \
  --sku S1 \
  --is-linux true
```

---

### 18.4 Create Web App

```bash
az webapp create \
  --name az204-demo-webapp \
  --resource-group rg-az204-appservice \
  --plan asp-az204-linux \
  --runtime "DOTNET|8.0"
```

---

### 18.5 Configure App Settings

```bash
az webapp config appsettings set \
  --resource-group rg-az204-appservice \
  --name az204-demo-webapp \
  --settings ASPNETCORE_ENVIRONMENT=Production
```

---

### 18.6 Enable Managed Identity

```bash
az webapp identity assign \
  --name az204-demo-webapp \
  --resource-group rg-az204-appservice
```

---

### 18.7 Create Deployment Slot

```bash
az webapp deployment slot create \
  --name az204-demo-webapp \
  --resource-group rg-az204-appservice \
  --slot staging
```

---

### 18.8 Scale Out

```bash
az webapp scale \
  --name az204-demo-webapp \
  --resource-group rg-az204-appservice \
  --number-of-workers 3
```

---

### 18.9 Azure PowerShell Example

```powershell
Connect-AzAccount

New-AzResourceGroup -Name rg-az204-appservice -Location EastUS

New-AzAppServicePlan \
  -Name asp-az204-win \
  -ResourceGroupName rg-az204-appservice \
  -Location EastUS \
  -Tier Standard

New-AzWebApp \
  -Name az204-demo-webapp \
  -ResourceGroupName rg-az204-appservice \
  -AppServicePlan asp-az204-win
```

---

**End of Azure
