
# 📄 ViewData in ASP.NET Core MVC

## 🔍 What is ViewData?

**ViewData** is a dictionary (`ViewDataDictionary`) that allows you to pass data from a **controller** to a **view** in ASP.NET Core MVC. It uses a key-value pair structure and is **loosely typed**, meaning values are accessed using string keys.

```csharp
ViewData["Key"] = value;
```

In the view:
```razor
@ViewData["Key"]
```

---

## 🛠️ Syntax

### In Controller:
```csharp
public IActionResult Index()
{
    ViewData["Message"] = "Hello from Controller!";
    ViewData["Count"] = 5;
    return View();
}
```

### In View (Index.cshtml):
```razor
<h2>@ViewData["Message"]</h2>
<p>Count: @ViewData["Count"]</p>
```

---

## ⚙️ Characteristics

| Feature              | Description |
|----------------------|-------------|
| Type                 | `ViewDataDictionary` |
| Namespace            | `Microsoft.AspNetCore.Mvc.ViewFeatures` |
| Storage Format       | Key-Value (string-object) |
| Scope                | Valid only for current HTTP request |
| Requires Casting     | ✅ Yes (explicit casting if used as object) |
| Null Safety          | ⚠️ Can throw if key doesn't exist |

---

## 🔁 ViewData vs ViewBag vs TempData

| Feature     | ViewData                         | ViewBag                        | TempData                          |
|-------------|----------------------------------|--------------------------------|-----------------------------------|
| Type        | Dictionary (string, object)      | Dynamic                        | Dictionary (string, object)       |
| Typing      | Loosely typed                    | Dynamic (wrapper over ViewData)| Loosely typed                     |
| Lifetime    | Current request only             | Current request only           | Survives across redirects         |
| Null Safety | Can be null if key doesn't exist | Can be null if key doesn't exist | Can be null if not accessed properly |
| Use case    | Pass data to views               | Pass data to views             | Pass data between actions         |

---

## 🚫 When Not to Use ViewData

- When passing **complex objects or lists** (prefer strongly-typed models or ViewModels)
- When **type safety** is important (prefer ViewModels)
- When needing to share data across multiple requests (use TempData or Session)

---

## ✅ When to Use ViewData

- For simple key-value pair passing (like a page title or message)
- When you don’t want to create a ViewModel just for a small piece of info

---

## 📝 Summary

- `ViewData` is a useful way to pass small pieces of data from a controller to a view.
- It is loosely typed and not type-safe.
- Best for lightweight, non-critical data transfer for the current request.
