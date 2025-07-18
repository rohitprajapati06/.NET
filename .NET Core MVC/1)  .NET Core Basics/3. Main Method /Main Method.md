# ASP.NET Core Main Method

In this article, we will discuss the **ASP.NET Core Main Method** with examples. We'll explore the role of the `Program.cs` class and the `Main` method in ASP.NET Core (.NET 8) applications.

---

## 🧠 Understanding `Program.cs` Class File in ASP.NET Core

When you open the `Program.cs` file in an ASP.NET Core project, you'll see a few lines of code that handle:

- Web server setup  
- Service configuration  
- Application startup to listen for HTTP requests  

### 📌 Top-Level Statements Feature

- If you **uncheck** "Do not use top-level statements" during project creation, you won't see the explicit `Program` class or `Main` method.
- This feature, introduced in **C# 9**, allows writing one "top-level" statement where the file itself acts as the application's entry point.

### 🕰 Historical Context

- Before .NET 6, ASP.NET Core used two separate files:
  - `Program.cs` – for host configuration
  - `Startup.cs` – for services and middleware
- From **.NET 6 onwards**, these files are merged into a single `Program.cs` file for simplicity.

---

## 🧱 Program Class and Main Method

### Program Class

- The main class of every .NET application  
- Serves as the entry point

### Main Method

- First method executed when the application runs  
- Static method (doesn't require an object of the class)  
- Accepts command-line arguments via `string[] args`

---

## 🚀 The Four Key Tasks of the `Main` Method

### ✅ Step 1: Creating the Web Host and Configuring Services

```csharp
var builder = WebApplication.CreateBuilder(args);
```

- Creates a `WebApplicationBuilder` instance  
- `args` parameter processes command-line arguments  
- Configures essential services with defaults:
  - Web server (Kestrel or IIS)
  - Hosting model (InProcess or OutOfProcess)
  - Logging (debug, console, etc.)
  - Configuration (JSON files, CLI, environment)
  - Dependency Injection container

---

### ✅ Step 2: Building the Application

```csharp
var app = builder.Build();
```

- Builds the actual `WebApplication` instance  
- The app is now ready to:
  - Set up routes
  - Configure middleware
  - Handle HTTP requests

---

### ✅ Step 3: Setting Up Endpoints, Routing, and Middleware

```csharp
app.MapGet("/", () => "Hello World!");
```

- Defines a simple HTTP GET endpoint

**Parameters:**

- `"/"` → route pattern (root URL)  
- `() => "Hello World!"` → request delegate returning plain text

This line returns **"Hello World!"** for GET requests to the root URL.

---

### ✅ Step 4: Running the Application

```csharp
app.Run();
```

- Starts the web server  
- Begins listening for incoming HTTP requests  
- Keeps the app alive until manually stopped

---

## 🧪 Complete Example Code

```csharp
namespace FirstCoreWebApplication
{
    public class Program
    {
        // Entry point to the application
        public static void Main(string[] args)
        {
            // Step 1: Creating the Web Host and Services
            var builder = WebApplication.CreateBuilder(args);
            
            // Step 2: Building the Application
            var app = builder.Build();
            
            // Step 3: Setting Up Endpoints, Routing and Middleware
            app.MapGet("/", () => "Hello World!");
            
            // Step 4: Running the Application
            app.Run();
        }
    }
}
```

---

## 🌐 Understanding `MapGet` Method

- Handles **HTTP GET** requests to specified routes

**Example behavior:**

- **Request**: `GET /`
- **Response**: `"Hello World!"`

For non-GET requests or unmatched URLs → `HTTP 404 Not Found`

---

## 🔍 Verification Tips

1. Run the application
2. Open browser developer tools (F12)
3. Go to the **Network** tab
4. Look for the following details:
   - **Request URL**: `https://localhost:7279/`
   - **Request Method**: `GET`

---
