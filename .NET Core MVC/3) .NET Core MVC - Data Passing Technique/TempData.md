
# TempData in ASP.NET Core MVC

`TempData` is a storage mechanism in ASP.NET Core MVC that allows data to persist between **two consecutive requests**, such as during a redirect.

---

## 🔑 Key Features

- Based on **Session**.
- Stores data **temporarily** until it is read.
- Automatically removed after one read unless **preserved**.
- Useful for passing messages across **redirects**.

---

## 🧪 Example

### Controller Code

```csharp
public class StudentController : Controller
{
    public IActionResult Create(Student student)
    {
        // Save logic here...

        TempData["SuccessMessage"] = "Student created successfully!";
        return RedirectToAction("Index");
    }

    public IActionResult Index()
    {
        ViewBag.Message = TempData["SuccessMessage"];
        return View();
    }
}
```

### View Code (Index.cshtml)

```html
@if (ViewBag.Message != null)
{
    <div class="alert alert-success">
        @ViewBag.Message
    </div>
}
```

---

## 🧠 How It Works

- **TempData["key"]**: Stores or retrieves the value.
- **TempData.Keep("key")**: Keeps the value for the next request.
- **TempData.Peek("key")**: Reads without removing.

---

## ⚙️ Common Methods

| Method | Description |
|--------|-------------|
| `TempData["key"]` | Get or set a value |
| `TempData.Peek("key")` | Read without removal |
| `TempData.Keep("key")` | Keep value for next request |

---

## 🛑 Limitations

- Relies on **Session**, so session state must be enabled.
- Not suitable for **long-term** storage.
- Not ideal for **large objects**.

---

## ✅ Use Cases

- Flash messages (success, error, warning)
- Redirect message passing
- Short-lived flags

---

## 🧾 Summary

| Feature       | TempData             |
|---------------|----------------------|
| Lifetime      | One request (unless kept) |
| Backed by     | Session              |
| Use Case      | Redirect messages, alerts |
| Access in View| Yes (via ViewBag or directly) |

---

> ✅ Tip: Prefer `ViewBag` or `ViewData` for passing data within a single request. Use `TempData` when redirecting.

