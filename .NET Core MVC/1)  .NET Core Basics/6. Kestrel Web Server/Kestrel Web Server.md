# Kestrel Web Server in ASP.NET Core Application


---

## What is Kestrel Web Server?

ASP.NET Core is a cross-platform framework, supporting development and execution on Windows, Linux, and macOS. Kestrel is the default cross-platform web server developed by Microsoft for hosting ASP.NET Core applications. It is:

- Lightweight
- Fast
- Efficient

Kestrel is suitable for both development and production environments.

---

## What is the Process Name for the Kestrel Web Server?

### Self-Contained Deployment

- Includes the .NET runtime.
- Generates an executable specific to the app (e.g., `FirstCoreWebApplication.exe`).
- Process Name = Name of the executable (e.g., `FirstCoreWebApplication`).

**Note:** Self-contained deployments offer portability but result in a larger size.

### Framework-Dependent Deployment

- Relies on shared .NET runtime installed on the host.
- Runs via `dotnet` (e.g., `dotnet.exe` on Windows).

**Note:** Smaller package, but .NET runtime must exist on the host machine.

---

## Running Applications Using Kestrel Web Server

Check the `launchSettings.json` file inside the **Properties** folder:

```
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:16145",
      "sslPort": 44335
    }
  },
  "profiles": {
    "http": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "applicationUrl": "http://localhost:5051",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "applicationUrl": "https://localhost:7084;http://localhost:5051",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

### IIS Express Profile

- Uses IIS Express.
- Application runs in `iisexpress.exe` process.
- Simulates production IIS environment.

### HTTP and HTTPS Profile

- Uses **Kestrel Web Server**.
- Runs in the application's own process.
- Good for cross-platform and reverse-proxy scenarios.

### WSL Profile

- Runs under **Windows Subsystem for Linux**.
- Useful for testing Linux production environments.

---

## Example to Display Worker Process Name

Update the `Program.cs` file:

```csharp
namespace FirstCoreWebApplication
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);
            var app = builder.Build();
            app.MapGet("/", () => "Worker Process Name : " + System.Diagnostics.Process.GetCurrentProcess().ProcessName);
            app.Run();
        }
    }
}
```

---

## Running the Application

### Using IIS Express

- Uses the `iisSettings` section in `launchSettings.json`.
- Output: `iisexpress.exe` is the worker process.

### Using Kestrel (HTTP/HTTPS Profile)

- Command prompt is launched with the Kestrel server.
- Application listens to the URL defined under `applicationUrl`.
- Verify using browser developer tools – server name is **Kestrel**.

---

## Hosting Model Used by Kestrel

When Kestrel handles requests directly **without** a reverse proxy, **InProcess** and **OutOfProcess** terms do **not apply**.

---

## Summary

- Kestrel is the default web server in ASP.NET Core.
- Supports cross-platform deployment.
- Can be used alone or behind a reverse proxy.
- Included by default with ASP.NET Core SDK.

---

