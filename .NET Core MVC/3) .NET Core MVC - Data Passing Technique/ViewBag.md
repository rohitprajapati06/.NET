
# 📄 ViewBag in ASP.NET Core MVC

## 🔍 What is ViewBag?

**ViewBag** is a dynamic wrapper around `ViewData` that allows you to pass data from a **controller** to a **view** using dynamic properties. It is a part of ASP.NET Core MVC and provides a more concise syntax compared to `ViewData`.

```csharp
ViewBag.PropertyName = value;
```

In the view:
```razor
@ViewBag.PropertyName
```

---

## 🛠️ Syntax

### In Controller:
```csharp
public IActionResult Index()
{
    ViewBag.Message = "Hello from ViewBag!";
    ViewBag.Count = 10;
    return View();
}
```

### In View (Index.cshtml):
```razor
<h2>@ViewBag.Message</h2>
<p>Count: @ViewBag.Count</p>
```

---

## ⚙️ Characteristics

| Feature              | Description |
|----------------------|-------------|
| Type                 | `dynamic` |
| Backed by            | `ViewData` |
| Storage Format       | Property-value (dynamic) |
| Scope                | Valid only for current HTTP request |
| Requires Casting     | ❌ No (since it's dynamic) |
| Null Safety          | ⚠️ Can be null if property not set |

---

## 🔁 ViewBag vs ViewData vs TempData

| Feature     | ViewData                         | ViewBag                        | TempData                          |
|-------------|----------------------------------|--------------------------------|-----------------------------------|
| Type        | Dictionary (string, object)      | Dynamic                        | Dictionary (string, object)       |
| Typing      | Loosely typed                    | Dynamic                        | Loosely typed                     |
| Lifetime    | Current request only             | Current request only           | Survives across redirects         |
| Null Safety | Can be null if key doesn't exist | Can be null if property not set | Can be null if not accessed properly |
| Use case    | Pass data to views               | Pass data to views             | Pass data between actions         |

---

## 🚫 When Not to Use ViewBag

- When **type safety** is needed
- When passing **complex models** or requiring **validation**
- When sharing data across requests (use TempData or Session)

---

## ✅ When to Use ViewBag

- For quick prototyping or small data like page titles, status messages
- When you prefer cleaner syntax over `ViewData["Key"]`

---

## 📝 Summary

- `ViewBag` is a convenient way to send small, simple pieces of data from controller to view.
- It is built on top of `ViewData`, but uses dynamic typing and property-based syntax.
- It shares the same request-lifetime scope as `ViewData`, but with less verbosity.

