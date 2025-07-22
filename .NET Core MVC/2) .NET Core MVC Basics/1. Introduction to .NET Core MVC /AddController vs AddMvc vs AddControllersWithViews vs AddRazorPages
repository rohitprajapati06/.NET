
# 📘 `AddController` vs `AddMvc` vs `AddControllersWithViews` vs `AddRazorPages` in ASP.NET Core

## 🔧 Overview

In ASP.NET Core, the `IServiceCollection` extension methods used during application startup determine the type of features and middleware support your app will have.

| Method | Purpose | Typical Usage |
|--------|---------|----------------|
| `AddControllers()` | Adds support for **Web API (controllers only)** | API-only apps |
| `AddMvc()` | Adds **full MVC support** including controllers, views, Razor Pages, model binding, etc. | Legacy or full MVC apps |
| `AddControllersWithViews()` | Adds support for **controllers and views**, but **not Razor Pages** | MVC apps without Razor Pages |
| `AddRazorPages()` | Adds support for **Razor Pages only** | Razor Pages-only apps |

---

## 📌 `AddControllers()`

- ✅ Registers services required for **controller-based APIs**.
- ❌ Does **not** support Views or Razor Pages.
- 🚀 Lightest of all options — suitable for **REST APIs**.

```csharp
builder.Services.AddControllers();
```

Use with:

```csharp
app.MapControllers();
```

✅ Example:  
`/api/products` → Returns JSON.

---

## 📌 `AddControllersWithViews()`

- ✅ Registers **Controllers + Views** (MVC).
- ❌ Does **not** include Razor Pages.
- Ideal when building web apps with **MVC pattern** but **no Razor Pages**.

```csharp
builder.Services.AddControllersWithViews();
```

Use with:

```csharp
app.MapDefaultControllerRoute();
```

✅ Example:  
`/Home/Index` → Returns an HTML View.

---

## 📌 `AddRazorPages()`

- ✅ Registers services for **Razor Pages**.
- ❌ Does **not** register MVC-style controllers.

```csharp
builder.Services.AddRazorPages();
```

Use with:

```csharp
app.MapRazorPages();
```

✅ Example:  
`/Index.cshtml` → Razor Page rendering.

---

## 📌 `AddMvc()`

- ✅ Adds **all** features: Controllers, Views, Razor Pages, API formatters, and more.
- 🧱 The **heaviest** and most inclusive option.
- 🧑‍💻 Used for backward compatibility or when you want everything.

```csharp
builder.Services.AddMvc();
```

Use with:

```csharp
app.MapDefaultControllerRoute();
app.MapRazorPages();
```

⚠️ Overkill for simple scenarios like APIs.

---

## ✅ Summary Table

| Feature/Service       | AddControllers | AddControllersWithViews | AddRazorPages | AddMvc |
|------------------------|----------------|--------------------------|----------------|--------|
| Web API Controllers    | ✅              | ✅                        | ❌              | ✅      |
| Views (.cshtml)        | ❌              | ✅                        | ❌              | ✅      |
| Razor Pages            | ❌              | ❌                        | ✅              | ✅      |
| Model Binding          | ✅              | ✅                        | ✅              | ✅      |
| Tag Helpers            | ❌              | ✅                        | ✅              | ✅      |
| Minimum Performance Load | 🟢 Lightest     | ⚪ Moderate                | ⚪ Moderate      | 🔴 Heaviest |

---

## 📝 Recommendations

| Scenario                          | Recommended Method             |
|----------------------------------|--------------------------------|
| Building a pure Web API          | `AddControllers()`             |
| MVC with Views (no Razor Pages)  | `AddControllersWithViews()`    |
| Razor Pages-only application     | `AddRazorPages()`              |
| Full-featured app with MVC + Razor Pages | `AddMvc()`                |
