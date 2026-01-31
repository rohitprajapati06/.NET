# Azure API Management (APIM) – AZ-204 Complete Guide

> **Purpose**: Detailed notes for **Azure Developer Associate (AZ-204)** exam preparation.
> This document is **GitHub-ready** and includes **architecture diagrams, policy examples, and exam tips**.

---

## 1. What is Azure API Management?

Azure API Management (APIM) is a **fully managed PaaS service** that allows organizations to **publish, secure, transform, monitor, and manage APIs**.

It acts as a **gateway** between API consumers and backend services.

![Azure API Management Architecture](https://learn.microsoft.com/en-us/azure/api-management/media/api-management-key-concepts/api-management-components.png)

### Core Components

| Component        | Description                           |
| ---------------- | ------------------------------------- |
| API Gateway      | Entry point for all API calls         |
| Management Plane | Admin operations via Portal, ARM, CLI |
| Developer Portal | API documentation and testing         |
| Analytics        | Logs, metrics, diagnostics            |

---

## 2. Create an Azure API Management Instance

### Creation Methods

* Azure Portal
* Azure CLI
* ARM / Bicep templates
* Terraform

### Required Parameters

* Resource Group
* Region
* Organization Name
* Administrator Email
* Pricing Tier

### Pricing Tiers (Exam Focus)

| Tier        | Use Case                           |
| ----------- | ---------------------------------- |
| Developer   | Non-production, testing            |
| Basic       | Low traffic production             |
| Standard    | Medium traffic                     |
| Premium     | VNET, multi-region (EXAM FAVORITE) |
| Consumption | Serverless, pay-per-call           |

⚠️ **Exam Tip**: VNET integration is supported **only in Premium tier**.

---

## 3. Create and Document APIs

### Ways to Create APIs

* Import OpenAPI (Swagger)
* Import Azure Functions
* Import App Service APIs
* Blank API
* SOAP to REST

![Create API in APIM](https://learn.microsoft.com/en-us/azure/api-management/media/api-management-howto-create-apis/api-management-create-api.png)

### API Structure

```
API
 ├── Operations (GET, POST, PUT, DELETE)
 │     └── Policies
 ├── Versions
 └── Revisions
```

### API Versioning vs Revisions

| Feature  | Purpose              |
| -------- | -------------------- |
| Version  | Breaking changes     |
| Revision | Non-breaking updates |

---

## 4. Developer Portal & Documentation

The **Developer Portal** provides:

* Interactive API documentation
* Try-it console
* Code samples
* Subscription management

![APIM Developer Portal](https://learn.microsoft.com/en-us/azure/api-management/media/api-management-howto-developer-portal/api-management-portal-overview.png)

---

## 5. Configure Access to APIs

### Products

A **Product** bundles APIs and defines access rules.

| Product Feature       | Description              |
| --------------------- | ------------------------ |
| APIs                  | APIs included in product |
| Subscription Required | Yes / No                 |
| Approval              | Manual or automatic      |
| Policies              | Rate limits, quotas      |

### Subscriptions

* Primary and Secondary keys
* Passed via:

  * Header: `Ocp-Apim-Subscription-Key`
  * Query string

⚠️ **Exam Tip**: Subscription keys control **access**, not authentication.

---

## 6. Authentication & Authorization

### Supported Methods

| Method              | Use Case               |
| ------------------- | ---------------------- |
| Subscription Key    | Default access control |
| OAuth 2.0           | Azure AD / Entra ID    |
| JWT Validation      | Token-based security   |
| Client Certificates | B2B scenarios          |

![APIM OAuth Flow](https://learn.microsoft.com/en-us/azure/api-management/media/api-management-howto-protect-backend-with-aad/api-management-oauth2-flow.png)

---

## 7. Implement Policies for APIs (MOST IMPORTANT)

![APIM Policies Flow](https://learn.microsoft.com/en-us/azure/api-management/media/api-management-policies/api-management-policy-flow.png)

### Policy Scopes

1. Global
2. Product
3. API
4. Operation

### Policy Sections

```xml
<policies>
  <inbound />
  <backend />
  <outbound />
  <on-error />
</policies>
```

### Common AZ-204 Policies

| Policy         | Purpose                  |
| -------------- | ------------------------ |
| rate-limit     | Throttling               |
| quota          | Usage limits             |
| validate-jwt   | Token validation         |
| rewrite-uri    | URL transformation       |
| set-header     | Modify headers           |
| cors           | Cross-origin support     |
| cache-response | Performance optimization |

### Example: Rate Limit Policy

```xml
<rate-limit calls="10" renewal-period="60" />
```

### Example: JWT Validation Policy

```xml
<validate-jwt header-name="Authorization">
  <openid-config url="https://login.microsoftonline.com/{tenant-id}/v2.0/.well-known/openid-configuration" />
  <required-claims>
    <claim name="aud">
      <value>api://client-id</value>
    </claim>
  </required-claims>
</validate-jwt>
```

---

## 8. Monitoring & Diagnostics

APIM integrates with:

* Azure Monitor
* Application Insights
* Log Analytics

Metrics include:

* Request count
* Latency
* Errors
* Throttled calls

---

## 9. AZ-204 Exam Key Takeaways

* APIM is a **gateway**, not a load balancer
* Policies solve **security, throttling, transformation**
* Premium tier enables **VNET & multi-region**
* Products + subscriptions control API exposure
* JWT + OAuth are frequently tested

---

## 10. Summary

Azure API Management is a **core AZ-204 topic** that combines:

* API security
* Governance
* Monitoring
* Scalability

Mastering **policies and access control** is essential for passing the exam.

---

