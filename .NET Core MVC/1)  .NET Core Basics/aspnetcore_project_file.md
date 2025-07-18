# ASP.NET Core Project File (.csproj) - Explained

This document provides an in-depth explanation of the ASP.NET Core Project File in a .NET Core MVC application. It is targeted at developers familiar with previous ASP.NET versions or those new to the .csproj structure in .NET Core.

---

## 📄 What is the ASP.NET Core Project File?

When you create a new ASP.NET Core Web Application (MVC) using Visual Studio, a project file with a `.csproj` extension is created. This file defines build configurations, dependencies, targets, and project metadata.

**Example:** If your project is named `MyWebApp`, the project file will be `MyWebApp.csproj`.

---

## ✅ Editing the Project File

Unlike older ASP.NET Framework versions, .NET Core allows direct editing of the project file without unloading the project:

**Steps:**

1. Right-click the project in Solution Explorer.
2. Click **Edit Project File**.
3. The `.csproj` file opens directly in the Visual Studio editor.

---

## 🧱 Structure of .csproj File

Here is a sample structure of a .NET Core MVC project file:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
  </ItemGroup>

</Project>
```

### 🔹 `<Project Sdk="Microsoft.NET.Sdk.Web">`

- Indicates it's a web project and uses the ASP.NET Core web SDK.
- Includes tooling, configurations, and packages required for web development.

### 🔹 `<PropertyGroup>`

Defines project-level settings:

- `` – Specifies the version of the .NET runtime (`net8.0`).
- `` – Enables nullable reference types to prevent null reference exceptions.
- `` – Automatically includes common `using` directives like `System`, `Microsoft.AspNetCore`, etc.

### 🔹 `<ItemGroup>`

Contains grouped items like package references, project references, etc.

- `` – Adds NuGet packages to your project.
  ```xml
  <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
  ```

---

## 📦 Adding Packages to the Project

Packages in ASP.NET Core are added via NuGet and automatically reflected in:

- The Dependencies node in Solution Explorer
- The `<ItemGroup>` of the `.csproj` file

**Steps to Add a Package:**

1. Go to: `Tools > NuGet Package Manager > Manage NuGet Packages for Solution...`
2. Install a package (e.g., `Newtonsoft.Json`)
3. The reference will be added to both Dependencies and `.csproj`

**To Uninstall a Package:**

- Go to the Installed tab in NuGet Package Manager and uninstall it. The reference is removed from both the UI and `.csproj`.

---

## 🗂 ASP.NET Core MVC Project – Default Files and Folders

| File/Folder                              | Description                                                                                    |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `Program.cs`                             | Entry point of the app; configures middleware and HTTP pipeline.                               |
| `Startup.cs` *(if using older versions)* | Used to configure services and middleware (replaced by `Program.cs` in minimal hosting model). |
| `Controllers/`                           | Contains MVC controller classes.                                                               |
| `Models/`                                | Stores application models or business logic classes.                                           |
| `Views/`                                 | Holds Razor views (`.cshtml` files).                                                           |
| `wwwroot/`                               | Public web assets like JS, CSS, and images.                                                    |
| `appsettings.json`                       | Stores app configuration like DB strings, logging, etc.                                        |
| `Properties/launchSettings.json`         | Local dev settings, e.g., URLs and environment profiles.                                       |

---

## 💡 Key Takeaways

- The `.csproj` file in .NET Core is XML-based, editable in-place, and uses a simplified structure.
- Modular: Only required packages are added.
- Automatically handles references and build targets via SDK-style projects.

---

In the next section/article, we will explore the **Main Method and Hosting Model** in ASP.NET Core applications.

