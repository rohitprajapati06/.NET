# ASP.NET Core Main Method

In this article, we will discuss the **ASP.NET Core Main Method** with examples. We'll explore the role of the `Program.cs` class and the `Main` method in ASP.NET Core (.NET 8) applications.

## Understanding Program.cs Class File in ASP.NET Core

When you open the `Program.cs` file in an ASP.NET Core project, you'll see a few lines of code that handle:
- Web server setup
- Service configuration
- Application startup to listen for HTTP requests

### Top-Level Statements Feature
- If you uncheck **"Do not use top-level statements"** during project creation, you won't see the explicit `Program` class or `Main` method.
- This feature, introduced in C# 9, allows writing one "top-level" statement where the file itself acts as the application's entry point.

### Historical Context
- Before .NET 6, ASP.NET Core used two separate files:
  - `Program.cs` (configured the host)
  - `Startup.cs` (handled services and middleware)
- From .NET 6 onward, these are merged into a single `Program.cs` file for simplicity.

---

## Program Class and Main Method

### Program Class
- The main class of every .NET application
- Serves as the entry point

### Main Method
- First method executed when the application runs
- Static method (doesn't require creating a Program class object)
- Accepts command-line arguments via `string[] args`

The main method performs four key tasks:
1. Creates the web host and configures services
2. Builds the application
3. Sets up endpoints, routing, and middleware
4. Runs the application

---

## Detailed Breakdown of the Main Method

### Step 1: Creating the Web Host and Configuring Services
```csharp
var builder = WebApplication.CreateBuilder(args);

Creates a WebApplicationBuilder instance

args parameter processes command-line arguments

Configures essential services with preconfigured defaults:

Web server setup (IIS or Kestrel)

Hosting model (InProcess or OutOfProcess)

Logging (debugging and console)

Configuration (access to config files)

Dependency Injection container

Step 2: Building the Application
csharp
var app = builder.Build();
Builds the actual WebApplication instance

The application is ready to:

Set up routes

Configure middleware

Handle requests (but not yet running)

Step 3: Setting Up Endpoints, Routing, and Middleware
csharp
app.MapGet("/", () => "Hello World!");
Defines a simple endpoint for HTTP GET requests

MapGet parameters:

Route pattern ("/" for root URL)

Request delegate (() => "Hello World!")

Returns "Hello World!" for GET requests to the root URL

Step 4: Running the Application
csharp
app.Run();
Starts the web server

Begins listening for incoming requests

Keeps the application running until manually stopped

Complete Example Code
csharp
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
Understanding MapGet Method
Handles HTTP GET requests to specified routes

Example behavior:

Request: GET /

Response: "Hello World!"

For non-GET requests or other URLs → HTTP 404 Not Found

Verification
Run the application

Open browser developer tools (F12)

Check Network tab:

Request URL: https://localhost:7279/

Request Method: GET
