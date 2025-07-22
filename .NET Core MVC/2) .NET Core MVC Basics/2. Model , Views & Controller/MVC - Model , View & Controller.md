
# 🔍 ASP.NET Core MVC: Controllers, Models, and Views

## 🧭 1. Controllers

### ✅ What is a Controller?
A **Controller** is the component in the MVC pattern responsible for handling **user input**, interacting with **models**, and returning a **response** (usually a view or JSON).

It acts as the **coordinator** between the Model and the View.

### 🛠️ Responsibilities of a Controller
- Receives and processes incoming HTTP requests
- Performs operations using Models (business/data layer)
- Chooses a View to render the response

### 🧪 Example:
```csharp
public class ProductsController : Controller
{
    public IActionResult Index()
    {
        var products = GetAllProducts(); // fetch from DB or service
        return View(products); // pass model to view
    }

    private List<string> GetAllProducts()
    {
        return new List<string> { "Phone", "Laptop", "Tablet" };
    }
}
```

### 🧭 Routing and Actions
Controllers use routing (either **attribute routing** or **conventional routing**) to map URLs to action methods.

```csharp
[Route("products")]
public class ProductsController : Controller
{
    [HttpGet("list")]
    public IActionResult List() => View();
}
```

---

## 📦 2. Models

### ✅ What is a Model?
A **Model** represents the application's **data structure** and **business logic**. It is used to retrieve, store, and manipulate data, typically interacting with a database.

### 🧠 Model Types
- **ViewModel**: Specific to views; used for passing formatted data.
- **Domain Model**: Represents business entities (like `Product`, `User`, etc).
- **Data Model**: Maps directly to database tables (often used with Entity Framework).

### 🛠️ Responsibilities of a Model
- Define data properties
- Apply validation using Data Annotations
- Encapsulate business logic or data access logic

### 🧪 Example:
```csharp
public class Product
{
    public int Id { get; set; }

    [Required]
    public string Name { get; set; }

    [Range(1, 10000)]
    public decimal Price { get; set; }
}
```

---

## 🖼️ 3. Views

### ✅ What is a View?
A **View** is responsible for rendering the **UI**. It uses the **Razor syntax** (`.cshtml` files) to generate dynamic HTML content based on the data received from the controller.

### 🧠 View Types
- **Full Views**: Render full HTML pages
- **Partial Views**: Render reusable parts of pages (headers, footers, lists)
- **Layout Views**: Define a consistent layout (like master pages)

### 🛠️ Responsibilities of a View
- Render user interface with Razor and HTML
- Display data from the model
- Format the output

### 🧪 Example:
`Views/Products/Index.cshtml`
```razor
@model List<string>

<h2>Product List</h2>
<ul>
@foreach (var product in Model)
{
    <li>@product</li>
}
</ul>
```

### 🧩 Razor Syntax Quick Tips
- `@Model` → Refers to model passed by controller
- `@{ }` → C# code block
- `@if`, `@foreach` → C# flow control
- `Html.DisplayFor()`, `Html.EditorFor()` → HTML helpers

---

## ✅ Summary

| Component   | Responsibility                          | File Location             |
|-------------|------------------------------------------|---------------------------|
| Controller  | Handles requests & responses             | `/Controllers/`           |
| Model       | Defines data and business logic          | `/Models/`                |
| View        | Generates UI using Razor                 | `/Views/{Controller}/`    |

ASP.NET Core MVC separates concerns into Models, Views, and Controllers for **clean architecture**, **testability**, and **maintainability**.
