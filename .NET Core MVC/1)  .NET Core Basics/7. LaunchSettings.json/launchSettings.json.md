
# 📘 launchSettings.json in ASP.NET Core

The `launchSettings.json` file is an important configuration file used in ASP.NET Core applications during development. It defines how the application should be launched, including environment variables, command-line arguments, and profiles for different hosting environments.

---

## 📂 Location

The file is typically located in the `Properties` folder of an ASP.NET Core project:
```
/MyProject/Properties/launchSettings.json
```

---

## 🛠️ Purpose

`launchSettings.json` configures:
- The **launch profile** to be used when debugging.
- The **application URL(s)** for HTTP and HTTPS.
- **Environment variables**, such as `ASPNETCORE_ENVIRONMENT`.
- Whether to launch a browser automatically.
- Integration with **IIS Express**, **Kestrel**, or **Docker**.

---

## 📄 Sample `launchSettings.json`

```json
{
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "launchUrl": "weatherforecast",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "MyProject": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

---

## 🔍 Explanation of Properties

| Property              | Description |
|-----------------------|-------------|
| `profiles`            | Contains multiple launch profiles. Each defines how the app should start in different environments. |
| `commandName`         | Defines what will be used to launch the app: `Project`, `IISExpress`, `Executable`, or `Docker`. |
| `launchBrowser`       | If `true`, launches the browser automatically on start. |
| `launchUrl`           | The URL or route the browser should navigate to once launched. |
| `applicationUrl`      | The HTTP and HTTPS endpoints used by Kestrel during development. |
| `environmentVariables`| Key-value pairs that set environment variables. Commonly used for `ASPNETCORE_ENVIRONMENT`. |
| `dotnetRunMessages`   | If `true`, enables messages from `dotnet run`, helpful for seeing startup logs. (Optional) |

---

## 🧪 Launch Profile Types

| Type        | Description |
|-------------|-------------|
| `IISExpress`| Uses IIS Express for launching the app. Requires Visual Studio. |
| `Project`   | Uses `dotnet run` (Kestrel server) to launch the app. Default for CLI projects. |
| `Executable`| Allows you to run a custom executable instead of the ASP.NET app. |
| `Docker`    | Used when launching with Docker containers. |

---

## 🌍 Environment Variable: `ASPNETCORE_ENVIRONMENT`

This variable is crucial for determining which environment-specific configurations (like `appsettings.Development.json`) to use.

Typical values:
- `Development`
- `Staging`
- `Production`

---

## ✅ Tips

- **Only used during development** – it's ignored in production environments.
- When using Visual Studio, you can choose the active profile via the dropdown next to the run button.
- You can override environment variables via CLI or system environment settings.

---

## 🧼 Cleaning Up for Production

Before deploying your application:
- Ensure no sensitive data (like secrets or tokens) are hard-coded in `launchSettings.json`.
- Do not include this file in production builds or Docker images unless necessary.

---

## 📁 Git Ignore

Typically, `launchSettings.json` is **included** in version control since it’s needed for consistent dev setup across teams. However, if developers have their own setup, they may choose to `.gitignore` it.
