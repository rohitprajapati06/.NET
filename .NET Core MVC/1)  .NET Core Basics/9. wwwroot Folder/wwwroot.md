
# 📁 wwwroot Folder in ASP.NET Core

The `wwwroot` folder in an ASP.NET Core application is the default root for serving **static files** like HTML, CSS, JavaScript, images, fonts, and other assets directly to clients.

---

## 📂 Location

It is found in the root directory of your ASP.NET Core project:
```
/MyProject/wwwroot
```

---

## 🛠️ Purpose

The `wwwroot` folder serves as the **web root** of the project, where public assets are placed. Files inside `wwwroot` are accessible via URL by default.

Example:  
If you have a file `wwwroot/images/logo.png`, it can be accessed as:
```
https://localhost:5001/images/logo.png
```

---

## 📄 Typical Contents

| Folder / File     | Purpose |
|--------------------|---------|
| `css/`             | CSS files (e.g., `site.css`) |
| `js/`              | JavaScript files (e.g., `site.js`) |
| `images/`          | Static images (e.g., `logo.png`) |
| `lib/`             | Client-side libraries installed via LibMan (Library Manager) |
| `favicon.ico`      | Site icon displayed in browser tabs |
| `index.html`       | Optional entry-point HTML for SPA apps |

---

## 🚀 Static File Serving

ASP.NET Core only serves files from the `wwwroot` folder by default.

To enable static file serving, ensure the following middleware is added in `Program.cs` or `Startup.cs`:

```csharp
app.UseStaticFiles();
```

---

## 🧪 Custom Static File Folders

If you want to serve static files from folders outside `wwwroot`, use:

```csharp
app.UseStaticFiles(new StaticFileOptions
{
    FileProvider = new PhysicalFileProvider(
        Path.Combine(Directory.GetCurrentDirectory(), "MyStaticFiles")),
    RequestPath = "/StaticFiles"
});
```

Now a file `MyStaticFiles/image.png` can be accessed as:
```
https://localhost:5001/StaticFiles/image.png
```

---

## 🔐 Security Consideration

- Do **not** place sensitive files (e.g., `.cs`, `.cshtml`, `.json`, `.dll`) inside `wwwroot` because it is publicly accessible.
- Restrict file types using `StaticFileOptions` if needed.

---

## ✅ Best Practices

- Store only static, non-sensitive resources.
- Organize by type (`css/`, `js/`, `images/`, etc.) for clarity.
- Use a bundler/minifier (like Webpack, Gulp) to manage large assets.
- Use LibMan or npm for third-party libraries in `wwwroot/lib`.

---

## 📁 Summary

- `wwwroot` is the web root where static files are kept.
- Files in it are accessible to the public via browser.
- Use `app.UseStaticFiles()` to enable static file serving.
