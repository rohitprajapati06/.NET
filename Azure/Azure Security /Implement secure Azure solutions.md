
# Implement Secure Azure Solutions (AZ-204)

> **Purpose**: These notes are designed for **Azure Developer Associate (AZ-204)** exam preparation.
> This document can be directly pushed to **GitHub** as a `.md` file.

---

## 1. Secure App Configuration Data

Modern cloud applications must **separate configuration from code** and **protect sensitive values** such as connection strings, API keys, and secrets.

Azure provides **two primary services** for this purpose:
- Azure App Configuration
- Azure Key Vault

---

## 2. Azure App Configuration

### 2.1 What is Azure App Configuration?

Azure App Configuration is a **centralized service** for managing **non-secret application settings**.

### Typical Use Cases
- Feature flags
- Environment-based settings (Dev / QA / Prod)
- Application behavior toggles
- Connection strings **without secrets**

### Key Features
- Centralized configuration store
- Versioning and labels
- Dynamic refresh without redeploy
- Feature Management support
- Native integration with .NET, Java, Spring Boot

### Example Configuration Keys
```
AppSettings:Theme=Dark
AppSettings:MaxItems=100
FeatureFlags:BetaUI=true
```

### .NET Integration Example
```csharp
builder.Configuration.AddAzureAppConfiguration(options =>
{
    options.Connect("<AppConfig-Connection-String>")
           .Select("*", LabelFilter.Null);
});
```

> ⚠️ **Exam Tip**: Azure App Configuration is **NOT meant for secrets**.

---

## 3. Azure Key Vault

### 3.1 What is Azure Key Vault?

Azure Key Vault is a **secure secrets management service** used to store:
- Secrets (passwords, API keys)
- Cryptographic keys
- Certificates (SSL/TLS)

### What You Store in Key Vault
| Type | Examples |
|---|---|
| Secrets | DB passwords, API keys |
| Keys | Encryption keys |
| Certificates | SSL certificates |

### Security Features
- Hardware Security Module (HSM)
- RBAC & Access Policies
- Audit logging
- Soft delete & purge protection

---

## 4. Using Secrets from Azure Key Vault in .NET

### 4.1 Add NuGet Packages
```bash
dotnet add package Azure.Identity
dotnet add package Azure.Security.KeyVault.Secrets
```

### 4.2 Access Secret Using Managed Identity
```csharp
var client = new SecretClient(
    new Uri("https://<your-keyvault-name>.vault.azure.net/"),
    new DefaultAzureCredential());

KeyVaultSecret secret = await client.GetSecretAsync("DbPassword");
string password = secret.Value;
```

> ✅ No secrets in code  
> ✅ No connection strings stored locally

---

## 5. Certificates in Azure Key Vault

### Common Scenarios
- HTTPS for App Service
- Mutual TLS
- Secure API authentication

### Certificate Usage
- Store certificate in Key Vault
- Grant app access via Managed Identity
- Bind certificate to App Service

---

## 6. Managed Identities for Azure Resources

### 6.1 What is Managed Identity?

Managed Identity allows Azure resources to **authenticate securely** without storing credentials.

### Why Managed Identity?
- No passwords or secrets
- Automatic rotation
- Azure AD-backed authentication
- Strongly recommended for AZ-204

---

## 6.2 Types of Managed Identities

| Type | Description |
|---|---|
| System-assigned | Tied to a single resource |
| User-assigned | Reusable across resources |

---

## 6.3 Enable Managed Identity

### App Service / Function App
```
Azure Portal → Identity → System Assigned → ON
```

### Assign Permissions
- Go to Key Vault
- Access Control (IAM)
- Add role: **Key Vault Secrets User**
- Assign to Managed Identity

---

## 7. Managed Identity Authentication Flow

1. App requests access token from Azure AD
2. Azure AD validates Managed Identity
3. Token issued
4. App accesses Key Vault securely

---

## 8. Best Practices (EXAM FAVORITES)

✅ Use **Azure App Configuration** for non-secret settings  
✅ Use **Azure Key Vault** for secrets & certificates  
✅ Use **Managed Identity** instead of connection strings  
✅ Never store secrets in `appsettings.json`  
✅ Prefer RBAC over Access Policies  

---

## 9. Common AZ-204 Exam Scenarios

| Scenario | Correct Choice |
|---|---|
| Secure DB password | Azure Key Vault |
| Feature flags | Azure App Configuration |
| No secrets in code | Managed Identity |
| Certificate management | Key Vault |
| Secure Azure-to-Azure auth | Managed Identity |

---

## 10. Summary

Azure security relies on:
- **Separation of config and code**
- **Centralized secret storage**
- **Password-less authentication**

Mastering **Key Vault + App Configuration + Managed Identity** is **critical for AZ-204 success**.

---

> 📌 **You are exam-ready when you can explain why Managed Identity is better than storing secrets.**
