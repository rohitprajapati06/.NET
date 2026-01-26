# Azure Functions – Complete AZ-204 Notes

> **Purpose**: This document is written specifically for **AZ-204: Developing Solutions for Microsoft Azure**.
> Focus areas include **architecture, hosting plans, triggers & bindings, .NET isolated model, CLI commands, deployment, security, and exam traps**.

---

## 1. What is Azure Functions?

Azure Functions is a **serverless compute service** that enables you to run **event-driven code** without provisioning or managing infrastructure.

You write small units of code called **functions**, and Azure automatically handles:

* Server provisioning
* Scaling
* Availability
* Patching

### Key Characteristics

* Event-driven execution
* Stateless by default
* Automatic scaling
* Pay-per-execution (Consumption plan)

---

## 2. Core Architecture

### Logical Hierarchy

```
Subscription
 └── Resource Group
     └── Function App
         ├── Function 1
         ├── Function 2
         └── Function N
```

### Core Components

| Component    | Description                                 |
| ------------ | ------------------------------------------- |
| Function App | Logical container for one or more functions |
| Function     | A single unit of execution                  |
| Trigger      | Event that starts execution                 |
| Binding      | Declarative input/output connection         |
| Runtime      | Executes the function code                  |

**Exam Rule**: A function can have **only one trigger**, but **multiple bindings**.

---

## 3. Triggers (High Exam Weight)

A **trigger** defines how a function is invoked.

### Common Triggers

| Trigger       | Use Case                  |
| ------------- | ------------------------- |
| HTTP          | REST APIs, Webhooks       |
| Timer         | Scheduled jobs            |
| Queue Storage | Background processing     |
| Blob Storage  | File upload processing    |
| Service Bus   | Enterprise messaging      |
| Event Grid    | Event-based architectures |

---

## 4. Bindings (Input & Output)

Bindings provide a **declarative way** to connect functions to data sources.

### Binding Types

* **Input Binding** – Reads data
* **Output Binding** – Writes data

### Supported Services

* Azure Blob Storage
* Azure Queue Storage
* Azure Service Bus
* Azure Cosmos DB
* Event Hubs

**Benefit**: Reduces boilerplate SDK code.

---

## 5. Hosting Plans (Very Important)

### 5.1 Consumption Plan

* Auto scaling
* Cold starts
* Pay per execution
* Timeout: 5 min (default), 10 min (max)

### 5.2 Premium Plan

* No cold starts
* Pre-warmed instances
* VNET integration
* Unlimited execution duration

### 5.3 Dedicated (App Service) Plan

* Fixed cost
* Manual scaling
* Shares App Service resources

### Hosting Comparison

| Feature    | Consumption     | Premium | Dedicated |
| ---------- | --------------- | ------- | --------- |
| Cold Start | Yes             | No      | No        |
| Auto Scale | Yes             | Yes     | Manual    |
| VNET       | No              | Yes     | Yes       |
| Billing    | Execution-based | Hybrid  | Fixed     |

---

## 6. Execution Models (.NET)

### In-Process (Legacy)

* Runs inside Azure Functions runtime
* Faster startup
* Tightly coupled

### Isolated Worker Model (Recommended)

* Runs in a separate .NET process
* Better dependency isolation
* Full middleware control

**AZ-204 Preference**: .NET Isolated Worker

---

## 7. Creating Azure Functions (.NET Isolated)

### Prerequisites

```bash
dotnet --version
node --version
az --version
func --version
```

Install Functions Core Tools:

```bash
npm install -g azure-functions-core-tools@4 --unsafe-perm true
```

### Initialize Project

```bash
func init Az204Functions --worker-runtime dotnet-isolated
cd Az204Functions
```

Create HTTP Trigger:

```bash
func new --name GetUser --template "HTTP trigger"
```

---

## 8. Program.cs (Isolated Worker)

```csharp
using Microsoft.Extensions.Hosting;

var host = new HostBuilder()
    .ConfigureFunctionsWorkerDefaults()
    .Build();

host.Run();
```

---

## 9. HTTP Trigger (.NET Code)

```csharp
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.Functions.Worker.Http;
using Microsoft.Extensions.Logging;
using System.Net;

public class GetUser
{
    private readonly ILogger _logger;

    public GetUser(ILoggerFactory loggerFactory)
    {
        _logger = loggerFactory.CreateLogger<GetUser>();
    }

    [Function("GetUser")]
    public HttpResponseData Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", Route = "users/{id}")]
        HttpRequestData req,
        string id)
    {
        _logger.LogInformation("User id received: {id}", id);

        var response = req.CreateResponse(HttpStatusCode.OK);
        response.WriteString($"User Id: {id}");
        return response;
    }
}
```

---

## 10. local.settings.json (Local Only)

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated"
  }
}
```

**Never deploy this file to production**.

---

## 11. Queue Output Binding Example

```csharp
[Function("OrderProcessor")]
public async Task<HttpResponseData> Run(
    [HttpTrigger(AuthorizationLevel.Function, "post")] HttpRequestData req,
    [QueueOutput("orders")] IAsyncCollector<string> queue)
{
    var body = await new StreamReader(req.Body).ReadToEndAsync();
    await queue.AddAsync(body);

    var response = req.CreateResponse(HttpStatusCode.Accepted);
    response.WriteString("Order queued successfully");
    return response;
}
```

---

## 12. Timer Trigger (CRON)

```csharp
[Function("DailyCleanup")]
public void Run([TimerTrigger("0 0 2 * * *")] TimerInfo timer,
    FunctionContext context)
{
    var logger = context.GetLogger("DailyCleanup");
    logger.LogInformation("Cleanup executed at {time}", DateTime.UtcNow);
}
```

CRON Format:

```
{second} {minute} {hour} {day} {month} {day-of-week}
```

---

## 13. Durable Functions (Stateful)

### Orchestrator

```csharp
[Function("OrderOrchestrator")]
public async Task RunOrchestrator(
    [OrchestrationTrigger] TaskOrchestrationContext context)
{
    await context.CallActivityAsync("ValidateOrder", null);
    await context.CallActivityAsync("ChargePayment", null);
}
```

### Activity

```csharp
[Function("ValidateOrder")]
public void ValidateOrder()
{
    // validation logic
}
```

Durable Functions store state in **Azure Storage**.

---

## 14. Azure CLI Deployment

### Create Resource Group

```bash
az group create --name az204-rg --location centralindia
```

### Create Storage Account

```bash
az storage account create \
  --name az204storage123 \
  --resource-group az204-rg \
  --location centralindia \
  --sku Standard_LRS
```

### Create Function App

```bash
az functionapp create \
  --resource-group az204-rg \
  --consumption-plan-location centralindia \
  --runtime dotnet-isolated \
  --functions-version 4 \
  --name az204-func-demo \
  --storage-account az204storage123
```

### Deploy

```bash
func azure functionapp publish az204-func-demo
```

---

## 15. Security

### Authorization Levels

| Level     | Description        |
| --------- | ------------------ |
| Anonymous | No key required    |
| Function  | Function-level key |
| Admin     | Host-level key     |

### Best Practices

* Use Azure AD authentication
* Use Managed Identity
* Store secrets in Azure Key Vault

---

## 16. Monitoring & Logging

Integrated with **Application Insights**:

* Execution time
* Failures
* Dependencies
* Live metrics

```csharp
_logger.LogInformation("Execution started");
_logger.LogError("Error occurred");
```

---

## 17. AZ-204 Exam Traps

* Cold start → Premium Plan
* State required → Durable Functions
* One trigger per function
* Consumption plan timeout limits
* local.settings.json is never deployed

---

## 18. Final Revision Checklist

* Serverless & event-driven concepts
* Hosting plan comparison
* Triggers vs Bindings
* .NET isolated model
* CLI deployment
* Durable Functions patterns
* Security & monitoring

---

**End of Document**
