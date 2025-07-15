# ASP.NET Core Empty Project Template (.NET 8)

This document provides a comprehensive overview of creating an **ASP.NET Core Empty Web Application** using **Visual Studio 2022** with the **.NET 8 (LTS)** framework. The *Empty* template is ideal for developers who prefer a minimal setup and want full control over the application structure from the ground up.

---

## 📘 What is the ASP.NET Core Empty Template?

The **ASP.NET Core Empty** template is a bare-bones project setup that includes:

- The minimal file and folder structure
- No predefined **Controllers**, **Views**, **Razor Pages**, or **API endpoints**
- Only essential configuration and startup files

> This template is best suited for applications where you want to add components manually and customize everything according to your project needs.

---

## 🧱 Step-by-Step Project Creation

### Step 1: Select the Empty Template

1. Open **Visual Studio 2022**
2. Click on **Create a new project**
3. Search for and select **ASP.NET Core Empty**
4. Click **Next**

---

### Step 2: Configure Your New Project

In this window, you define the project's structure and location.

| Field                          | Description |
|-------------------------------|-------------|
| **Project name**              | The name of your project, e.g., `FirstCoreWebApplication`. This name is also used as the default namespace. |
| **Location**                  | The local folder path where your project files will be saved. E.g., `D:\Projects`. |
| **Solution name**             | The name of the solution file (`.sln`). Defaults to the project name if left unchanged. |
| **Place solution and project in the same directory** | If checked, both the `.sln` and project files will be placed in the same directory. Otherwise, Visual Studio creates a root solution folder with a nested project folder. |
| **Final Path**                | Example: `D:\Projects\FirstCoreWebApplication\FirstCoreWebApplication\` where the outer folder is the solution, and the inner is the project itself. |

Click **Next** to proceed to additional configurations.

---

### Step 3: Configure Additional Project Settings

This window allows you to specify runtime and deployment configurations.

| Setting                           | Description |
|-----------------------------------|-------------|
| **Framework**                     | Select **.NET 8 (Long-Term Support)**. This is a stable release suitable for production systems. |
| **Configure for HTTPS**           | ✅ Checked by default. Adds HTTPS support for secure communication. |
| **Enable Container Support**      | ❌ Leave unchecked unless you plan to containerize the application using Docker. |
| **Container OS**                  | If container support is enabled, select **Linux** (default) or **Windows**. Linux is lightweight and commonly used in cloud environments. |
| **Container Build Type**          | Defaults to **Dockerfile** (if enabled). Specifies how the container image will be built. |
| **Do not use top-level statements** | ✅ Check this to use the traditional `Program.cs` structure with an explicit `Main` method. |
| **Enlist in .NET Aspire Orchestration** | ❌ Leave unchecked unless deploying as part of a .NET Aspire orchestrated solution (advanced DevOps/Azure setup). |

Click **Create** to generate the project.

---

## 📂 Project Structure Overview

Once the project is created, you'll see the following structure:

