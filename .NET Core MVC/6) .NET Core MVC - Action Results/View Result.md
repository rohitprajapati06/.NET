
# 🖼️ ViewResult in ASP.NET Core MVC

## 📌 What is ViewResult?

`ViewResult` is a type of **Action Result** that renders a Razor view (i.e., `.cshtml` file) to the response. It is commonly used in ASP.NET Core MVC applications to return HTML content to the browser.

It is the default return type of controller action methods that return views.

---

## 🧱 ViewResult Class

Namespace: `Microsoft.AspNetCore.Mvc`  
Inheritance: `ViewResult : ActionResult`

```csharp
public class ViewResult : ActionResult
```

---

## ✅ When to Use ViewResult

Use `ViewResult` when:
- You want to return a Razor view from a controller.
- You want to pass data from the controller to the view using `ViewData`, `ViewBag`, or a model.

---

## 🧪 Basic Example

```csharp
public class HomeController : Controller
{
    public ViewResult Index()
    {
        return View(); // Returns Views/Home/Index.cshtml
    }
}
```

If you call `return View();` without parameters, ASP.NET Core looks for a `.cshtml` file matching the action name inside the `Views/<ControllerName>` folder.

---

## 🧬 View with Model Example

```csharp
public class ProductController : Controller
{
    public ViewResult Details()
    {
        var product = new Product { Id = 1, Name = "Coffee Mug", Price = 299 };
        return View(product);
    }
}
```

In the corresponding view:

```csharp
@model Product

<h1>@Model.Name</h1>
<p>Price: ₹@Model.Price</p>
```

---

## 📁 View with Custom Name and Folder

You can pass the view name and even a custom path:

```csharp
return View("CustomViewName");
return View("~/Views/Shared/About.cshtml");
```

---

## 🎒 Passing Data with ViewData & ViewBag

```csharp
public ViewResult Profile()
{
    ViewData["Name"] = "Rohit";
    ViewBag.Age = 25;
    return View();
}
```

In the Razor view:

```csharp
<h2>@ViewData["Name"]</h2>
<p>Age: @ViewBag.Age</p>
```

---

## 🔄 ViewResult vs IActionResult

```csharp
public IActionResult Index()
{
    return View(); // still returns ViewResult
}
```

Returning `IActionResult` allows flexibility (you can return other result types too), while `ViewResult` is specific to returning views.

---

## 📜 Summary

| Feature         | Description                              |
|----------------|------------------------------------------|
| `ViewResult`    | Renders and returns a Razor view         |
| Default         | If action returns `View()`               |
| Data Passing    | Supports `ViewData`, `ViewBag`, and model |
| Custom View     | Supports custom view name/path           |
| Flexible Use    | Can be returned via `IActionResult`      |

---

## 📚 Use Case

Use `ViewResult` when building MVC-style web pages with Razor Views and need full control over rendering HTML using data sent from a controller.
