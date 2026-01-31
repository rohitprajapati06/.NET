
# Implement Azure Security – AZ-204 Exam Notes

> **Exam Weight:** 15–20%  
> **Target Exam:** Microsoft Azure Developer Associate (AZ-204)  
> **Purpose:** Detailed notes for GitHub and revision

---

## 1. Implement User Authentication and Authorization

### Authentication vs Authorization

| Concept | Description |
|------|-------------|
| Authentication | Verifies *who* the user or app is |
| Authorization | Determines *what* the user or app can access |

Azure primarily uses **Microsoft Identity Platform** and **Microsoft Entra ID** for these purposes.

---

## 2. Microsoft Identity Platform

The Microsoft Identity Platform is a **developer platform** that enables apps to sign in users and acquire tokens to call protected APIs.

### Core Components

- Microsoft Entra ID (formerly Azure AD)
- OAuth 2.0
- OpenID Connect (OIDC)
- Microsoft Authentication Library (MSAL)
- JSON Web Tokens (JWT)

### Token Types

| Token | Purpose |
|------|--------|
| ID Token | User authentication |
| Access Token | API authorization |
| Refresh Token | Get new access tokens |

---

## 3. Microsoft Entra ID (Azure AD)

Microsoft Entra ID is a **cloud-based identity and access management service**.

### Supported Identities

- Work accounts (Entra ID)
- Microsoft personal accounts
- Social identities (B2C)
- Managed identities

### App Registration

Steps:
1. Register app in Entra ID
2. Configure redirect URI
3. Generate client secret / certificate
4. Assign API permissions

---

## 4. Authentication Flows (EXAM IMPORTANT)

### Authorization Code Flow (Most Common)

Used by:
- Web apps
- SPAs (with PKCE)

Flow:
1. User signs in
2. Authorization code returned
3. Code exchanged for token

### Client Credentials Flow

Used by:
- Daemons
- Background services

No user interaction.

---

## 5. Implement Authentication in .NET (ASP.NET Core)

```csharp
builder.Services.AddAuthentication("Bearer")
    .AddJwtBearer("Bearer", options =>
    {
        options.Authority = "https://login.microsoftonline.com/{tenant-id}/v2.0";
        options.TokenValidationParameters.ValidateAudience = false;
    });
```

Enable authorization:

```csharp
builder.Services.AddAuthorization();
```

Protect controller:

```csharp
[Authorize]
public IActionResult SecureEndpoint()
{
    return Ok("Authorized");
}
```

---

## 6. Managed Identities (VERY IMPORTANT FOR EXAM)

Managed Identity allows Azure services to authenticate **without storing secrets**.

### Types

| Type | Description |
|----|-------------|
| System-assigned | Tied to a single resource |
| User-assigned | Reusable across resources |

### Used To Access

- Azure Key Vault
- Azure SQL
- Storage Accounts
- Microsoft Graph

---

## 7. Shared Access Signatures (SAS)

SAS provides **delegated access** to Azure Storage resources.

### Types of SAS

| SAS Type | Scope |
|--------|-------|
| Service SAS | Blob / File / Queue |
| Account SAS | Entire storage account |
| User Delegation SAS | Uses Entra ID |

### Blob SAS Example

```csharp
var sasBuilder = new BlobSasBuilder
{
    BlobContainerName = "images",
    BlobName = "photo.png",
    Resource = "b",
    ExpiresOn = DateTimeOffset.UtcNow.AddMinutes(15)
};

sasBuilder.SetPermissions(BlobSasPermissions.Read);

var sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();
```

### Best Practices

- Short expiration
- Least privilege
- Prefer User Delegation SAS

---

## 8. Implement Solutions That Interact with Microsoft Graph

Microsoft Graph is a **REST API** that provides access to Microsoft 365 data.

### Common Use Cases

- Read users
- Manage groups
- Access calendars
- Retrieve profile info

### Required Permissions

| Type | Used When |
|-----|-----------|
| Delegated | User signed in |
| Application | Background apps |

### Graph SDK Example (.NET)

```csharp
var graphClient = new GraphServiceClient(
    new TokenCredentialAuthProvider(credential));

var users = await graphClient.Users.GetAsync();
```

### Required App Permissions

- User.Read
- User.Read.All
- Group.Read.All

(Admin consent required)

---

## 9. Security Best Practices (EXAM FAVORITE)

- Never store secrets in code
- Use Managed Identity
- Use Key Vault
- Use HTTPS only
- Validate JWT tokens
- Apply least privilege access

---

## 10. Exam Tips

- Managed Identity > Client Secrets
- Authorization ≠ Authentication
- SAS is for **temporary access**
- Microsoft Graph requires permissions + consent
- Entra ID secures apps, users, and APIs

---

## 11. Summary

This document covers:
- Authentication & Authorization
- Microsoft Identity Platform
- Microsoft Entra ID
- Managed Identity
- Shared Access Signatures
- Microsoft Graph Integration

---

**Prepared for AZ-204 Exam**
