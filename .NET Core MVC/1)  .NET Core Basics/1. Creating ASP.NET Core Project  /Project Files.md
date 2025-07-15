# 📁 ASP.NET Core MVC Project Structure (.NET 8)

This document outlines the default files and folders included when you create a new **ASP.NET Core MVC Project** using Visual Studio with the MVC template.

The MVC (Model-View-Controller) template provides a fully functional web application structure ready for rapid development of web apps using the MVC pattern.

---

## 🗂️ Project File & Folder Structure

| File/Folder                      | Description |
|----------------------------------|-------------|
| **Program.cs**                  | Entry point of the application. It sets up services (like MVC) and configures the HTTP request pipeline. |
| **Startup.cs** *(only in older versions)* | Not used in .NET 6/7/8. `Program.cs` handles startup configuration now. |
| **YourProject.csproj**          | Project configuration file that includes framework target, NuGet references, build settings, etc. |
| **Controllers/**                | Contains controller classes (e.g., `HomeController.cs`) that handle incoming HTTP requests and return responses. |
| **Views/**                      | Holds Razor View files (`.cshtml`) for rendering UI. Organized into folders per controller (e.g., `Views/Home/Index.cshtml`). |
| **Models/**                     | Contains model classes that define application data and business logic. |
| **wwwroot/**                    | The public web root. Stores static files such as CSS, JavaScript, and images accessible by the browser. |
| **appsettings.json**            | Stores configuration data like connection strings, logging, or custom settings. |
| **appsettings.Development.json**| Environment-specific configuration for development. |
| **Properties/**                 | Contains `launchSettings.json` for configuring local development settings like environment and URLs. |
| └── **launchSettings.json**     | Defines how the app runs locally, including environment variables and launch URLs. |
| **_ViewImports.cshtml**         | Allows importing common namespaces, tag helpers, and directives for all views. |
| **_ViewStart.cshtml**           | Runs before every view. Typically sets the layout file for consistent structure. |
| **Layouts/** *(inside `Views/Shared/`)* | Contains shared layout files like `_Layout.cshtml`, which define the common HTML structure. |
| **Error.cshtml** *(in `Views/Shared/`)* | Default error page displayed for exceptions. |
| **favicon.ico**                 | The default site icon shown in browser tabs. |
| **libman.json** *(optional)*    | If using Library Manager (LibMan), it manages client-side libraries like Bootstrap or jQuery. |

---

## ✅ Summary

The ASP.NET Core MVC template provides a complete scaffold for web applications using the **Model-View-Controller** design pattern.

| Layer       | Folder         | Purpose |
|-------------|----------------|---------|
| Model       | `Models/`      | Data and business logic |
| View        | `Views/`       | UI (Razor Pages) |
| Controller  | `Controllers/` | Request handling and routing logic |

This structure encourages **separation of concerns**, making it easier to maintain, test, and scale your application.

---

**Tip:**  
If you're new to MVC, focus on these files first:
- `HomeController.cs`
- `Views/Home/Index.cshtml`
- `_Layout.cshtml` (for consistent layout across pages)
- `Program.cs` (for pipeline and routing configuration)

