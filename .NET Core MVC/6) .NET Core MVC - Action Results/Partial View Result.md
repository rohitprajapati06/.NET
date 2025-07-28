# PartialViewResult in ASP.NET Core MVC

## 🧩 What is PartialViewResult?

`PartialViewResult` is a type of `ActionResult` in ASP.NET Core MVC used to return a **partial view**. Partial views are used to render a section of HTML that can be reused across different views. They are typically used to avoid code duplication and improve maintainability.

---

## 🛠️ Syntax

```csharp
public PartialViewResult PartialView(string viewName, object model);
```

Or simply:

```csharp
return PartialView(); // Returns a partial view with the same name as the action method.
```

---

## ✅ When to Use PartialViewResult

- When you want to render a piece of the HTML page that can be reused.
- When using AJAX to load only part of a page instead of a full-page reload.
- To split large views into manageable components.

---

## 📦 Example

### Controller:

```csharp
public class StudentController : Controller
{
    public IActionResult Details()
    {
        Student student = new Student { Id = 1, Name = "John", Branch = "CSE" };
        return PartialView("_StudentDetailsPartial", student);
    }
}
```

### Partial View (`_StudentDetailsPartial.cshtml`):

```html
@model Student

<div class="student-info">
    <h3>@Model.Name</h3>
    <p>Branch: @Model.Branch</p>
</div>
```

### Parent View:

```html
<div id="student-section">
    @Html.Partial("_StudentDetailsPartial", Model)
</div>
```

Or using AJAX:

```javascript
$.get("/Student/Details", function (data) {
    $("#student-section").html(data);
});
```

---

## 🧪 Return Type

In ASP.NET Core MVC, you can explicitly return a `PartialViewResult` like this:

```csharp
public PartialViewResult LoadDetails()
{
    return PartialView("_DetailsPartial", model);
}
```

This is functionally equivalent to:

```csharp
public IActionResult LoadDetails()
{
    return PartialView("_DetailsPartial", model);
}
```

---

## 🧠 Key Notes

- `PartialViewResult` only returns the **HTML fragment** without layout.
- It is best used in scenarios like **modals**, **components**, or **AJAX-based updates**.
- Partial views must be placed in the **Shared** folder or the corresponding **View folder** of the controller.