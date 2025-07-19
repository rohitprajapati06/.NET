
# ASP.NET Core InProcess Hosting Model

In this article, we will discuss the **ASP.NET Core InProcess Hosting Model** with examples.

## What is Hosting?

Hosting refers to the service that allows our web application to be accessible over the Internet. It involves deploying the application's files, databases, and resources on a server that can respond to requests from clients.

## What is a Web Server?

A web server is a machine (physical or virtual) running software like Apache, Nginx, or IIS that accepts HTTP requests, executes the logic, and sends back responses.

## Hosting Models in ASP.NET Core

ASP.NET Core provides two primary hosting models:

- **In-Process Hosting**
- **Out-of-Process Hosting**

These models define how ASP.NET Core interacts with web servers and how requests are processed.

---

## InProcess Hosting Model in ASP.NET Core

In this model, the application runs inside the **IIS worker process (`w3wp.exe`)**. It's the default for IIS-hosted apps and offers **better performance** by avoiding inter-process communication.

### OutOfProcess Hosting Model (Briefly)

In OutOfProcess, ASP.NET Core runs in a separate process using **Kestrel**, and IIS acts as a **reverse proxy** forwarding requests to Kestrel.

---

## How InProcess Hosting Works

### Step-by-Step Request Lifecycle:

1. **Client Request**: User makes an HTTP request via browser.
2. **IIS**:
   - If the app is not started, forwards request to **ASP.NET Core Module (ANCM)**.
   - If running, sends request directly to **IIS HTTP Server**.
3. **ASP.NET Core Module (ANCM)**:
   - Initializes the app inside `w3wp.exe`.
   - Starts .NET Core runtime if needed.
4. **IIS HTTP Server**:
   - Handles HTTP protocol, decodes request, sends to app.
5. **Application Code**:
   - Processes the request via Middleware pipeline (auth, routing, services).
   - Returns the response.
6. **IIS HTTP Server**:
   - Sends response to IIS.
7. **IIS**:
   - Delivers the response to the client.

> ANCM only handles startup, not every request.

---

## Example of InProcess Hosting in Program.cs

```csharp
namespace FirstCoreWebApplication
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);
            var app = builder.Build();

            app.MapGet("/", () =>
                "Worker Process Name : " + System.Diagnostics.Process.GetCurrentProcess().ProcessName);

            app.Run();
        }
    }
}
```

Run the app using **IIS Express**, and it will show the process name: `iisexpress`.

---

## Configure InProcess Hosting

### Method 1: Edit `.csproj` File

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
  </PropertyGroup>
</Project>
```

### Method 2: Via Visual Studio GUI

1. Right-click project > **Properties**
2. Go to **Debug** tab
3. Open **Launch Profiles UI**
4. Set Hosting Model to **InProcess**

---

## Benefits of InProcess Hosting

- **Performance**: No inter-process communication
- **Efficiency**: Application runs inside IIS process
- **Ideal for**: High-performance production scenarios on Windows

---

## What is IIS Express?

A lightweight local development version of IIS used for testing apps. **Used only in development**, not in production.

---

## Summary

- InProcess hosting runs ASP.NET Core apps inside `w3wp.exe`
- Better performance than OutOfProcess
- Configurable via `.csproj` or Visual Studio UI
- Used with IIS or IIS Express
- Ideal for Windows server hosting

---

