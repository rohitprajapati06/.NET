
# ASP.NET Core OutOfProcess Hosting Model

In this article, we will explore the **OutOfProcess Hosting Model** in ASP.NET Core with examples. This is one of the two hosting models supported by ASP.NET Core: **InProcess** and **OutOfProcess**.

---

## What is OutOfProcess Hosting?

In the **OutOfProcess hosting model**, the ASP.NET Core application runs in a separate process from the IIS worker process. The internal Kestrel web server is used to handle HTTP requests, and IIS acts as a **reverse proxy**, forwarding requests to the Kestrel server.

---

## Process Flow in OutOfProcess Hosting

1. **IIS** receives the HTTP request.
2. **IIS forwards** the request to **Kestrel** (which listens on a dynamic port).
3. **Kestrel** processes the request and sends back the response.
4. **IIS** returns the response to the client.

---

## Configuration in the Project File

To configure the hosting model, open your project file (`.csproj`) and modify the following property:

```xml
<AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
```

This can be explicitly set, although `OutOfProcess` is the default if the property is not specified.

---

## Web.config for OutOfProcess Hosting

In OutOfProcess, the `web.config` generated looks like this:

```xml
<aspNetCore processPath="dotnet" arguments="YourApp.dll" stdoutLogEnabled="false" hostingModel="OutOfProcess" />
```

- `processPath="dotnet"` indicates the .NET runtime is used.
- `arguments="YourApp.dll"` runs your app through the runtime.
- `hostingModel="OutOfProcess"` explicitly sets the model.

---

## Advantages of OutOfProcess Hosting

- More **cross-platform** compatibility (Kestrel works on Windows, Linux, macOS).
- Decouples application process from IIS.
- Better suited for **containers** and **Linux deployments**.

---

## Disadvantages

- Slight performance hit due to reverse proxy.
- Slightly more complex architecture.

---

## When to Use OutOfProcess Hosting

- When targeting **Linux servers**.
- When hosting **cross-platform applications**.
- When using **Docker containers**.
- When greater control over the web server (Kestrel) is needed.

---

## Program.cs (Typical OutOfProcess Setup)

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello from OutOfProcess Hosting!");

app.Run();
```

---

## Verifying the Hosting Model

1. Check the `web.config` file.
2. Or use Visual Studio:
   - Right-click the project → **Edit Project File**.
   - Locate the `<AspNetCoreHostingModel>` property.

---

## Summary

| Feature                     | InProcess                     | OutOfProcess                      |
|----------------------------|-------------------------------|-----------------------------------|
| Process                    | IIS Worker Process            | Separate `dotnet` process         |
| Performance                | Better                        | Slightly slower                   |
| Platform Compatibility     | Windows-only                  | Cross-platform                    |
| Uses Kestrel               | Optional (IIS handles requests)| Always uses Kestrel               |
| Use Case                   | High-performance Windows apps | Cross-platform and container apps |

---

