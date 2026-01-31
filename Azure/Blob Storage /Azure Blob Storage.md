
# Azure Blob Storage – AZ-204 Developer Notes

> **Purpose**: This document is designed for **AZ-204: Developing Solutions for Microsoft Azure** exam preparation.  
> It focuses on **application development**, SDK usage, security, and real-world scenarios.

---

## 1. What is Azure Blob Storage?

Azure Blob Storage is Microsoft Azure’s **object storage solution** for storing massive amounts of **unstructured data** such as images, videos, documents, logs, and backups.

### Common Use Cases
- File and media storage
- Backup and disaster recovery
- Static website hosting
- Log and telemetry storage
- Big data analytics

---

## 2. Blob Storage Architecture

### Hierarchy
```
Storage Account
 └── Container
      └── Blob
```

### Blob Types (EXAM IMPORTANT)

| Blob Type | Description | Use Case |
|----------|-------------|----------|
| Block Blob | Optimized for streaming & upload | Images, videos, files |
| Append Blob | Optimized for append-only writes | Logs |
| Page Blob | Random read/write | VHD files (VM disks) |

> **AZ-204 Tip**: Most application scenarios use **Block Blobs**.

---

## 3. Storage Account Types

| Type | Description |
|-----|------------|
| General-purpose v2 (GPv2) | Recommended, supports all features |
| BlobStorage | Legacy, limited features |

Always choose **GPv2** for modern applications.

---

## 4. Access Tiers (Cost Optimization)

| Tier | Usage Pattern | Cost |
|-----|---------------|------|
| Hot | Frequently accessed | Higher storage, low access |
| Cool | Infrequent (≥30 days) | Lower storage, higher access |
| Archive | Rare (≥180 days) | Lowest storage, highest access |

> Archive blobs must be **rehydrated** before reading.

---

## 5. Creating Blob Storage (Azure CLI)

```bash
az storage account create   --name mystorageaz204   --resource-group MyRG   --location eastus   --sku Standard_LRS   --kind StorageV2
```

---

## 6. Azure Blob Storage SDK (.NET)

### Install Package
```bash
dotnet add package Azure.Storage.Blobs
```

### Create BlobServiceClient
```csharp
using Azure.Storage.Blobs;

var connectionString = "<connection-string>";
BlobServiceClient serviceClient =
    new BlobServiceClient(connectionString);
```

---

## 7. Create Container and Upload Blob

```csharp
BlobContainerClient containerClient =
    serviceClient.GetBlobContainerClient("documents");

await containerClient.CreateIfNotExistsAsync();

BlobClient blobClient =
    containerClient.GetBlobClient("file1.txt");

await blobClient.UploadAsync("file1.txt", overwrite: true);
```

---

## 8. Download Blob

```csharp
await blobClient.DownloadToAsync("downloaded-file.txt");
```

---

## 9. Security & Access Control (EXAM FAVORITE)

### Azure AD + Managed Identity (Recommended)
- No secrets in code
- Uses RBAC

```csharp
BlobServiceClient client =
    new BlobServiceClient(
        new Uri("<blob-uri>"),
        new DefaultAzureCredential());
```

### Shared Access Signatures (SAS)
- Temporary, restricted access
- Ideal for client-side/browser access

```csharp
BlobSasBuilder sasBuilder = new BlobSasBuilder
{
    BlobContainerName = "documents",
    BlobName = "file1.txt",
    Resource = "b",
    ExpiresOn = DateTimeOffset.UtcNow.AddMinutes(30)
};

sasBuilder.SetPermissions(BlobSasPermissions.Read);
```

---

## 10. Security Features

- Encryption at rest (Microsoft-managed keys)
- Customer-managed keys (Key Vault)
- HTTPS enforced
- Private Endpoints
- Immutable blobs (WORM)

---

## 11. Lifecycle Management

Automatically move or delete blobs based on age.

### Example Policy
```json
{
  "rules": [
    {
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": { "blobTypes": ["blockBlob"] },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 365 }
          }
        }
      }
    }
  ]
}
```

---

## 12. Static Website Hosting

- Enable Static Website in storage account
- Uses `$web` container
- Hosts HTML, CSS, JS

---

## 13. Monitoring & Diagnostics

- Azure Monitor metrics
- Diagnostic logs
- Storage Analytics
- Alerts for latency and capacity

---

## 14. Performance Optimization

- Parallel uploads for large files
- Retry policies
- Use CDN for global access
- Correct tier selection

---

## 15. AZ-204 Exam Scenarios

| Scenario | Best Option |
|--------|-------------|
| Secure app access | Managed Identity |
| Temporary user access | SAS |
| Log storage | Append Blob |
| VM disks | Page Blob |
| Cost optimization | Lifecycle policies |

---

## 16. Key Takeaways

- Understand blob types and tiers
- Use SDK efficiently
- Prefer Managed Identity over keys
- Lifecycle policies save cost
- Security & performance are heavily tested

---

**Happy Learning & All the Best for AZ-204 🚀**
