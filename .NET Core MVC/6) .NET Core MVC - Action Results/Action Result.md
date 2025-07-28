
# 🎯 Action Results in ASP.NET Core MVC

## 📝 What is an Action Result?

In ASP.NET Core MVC, an **Action Result** represents the result of an action method in a controller. It is what the **controller returns to the client**.

Action results can be:
- **Views** (HTML)
- **Redirects**
- **JSON/XML data**
- **Status codes**
- **Files**

---

## 🔍 Base Class

All action results derive from the abstract class:

```csharp
Microsoft.AspNetCore.Mvc.ActionResult
```

You can return either:
- `IActionResult` (interface)
- `ActionResult<T>` (for Web APIs with strongly-typed data)

---

## 📂 Common Action Results

### 1. **ViewResult**
Returns a rendered HTML view.

```csharp
public IActionResult Index() {
    return View();
}
```

---

### 2. **JsonResult**
Returns JSON-formatted data.

```csharp
public JsonResult GetUser() {
    return Json(new { id = 1, name = "Rohit" });
}
```

---

### 3. **ContentResult**
Returns plain text, HTML, or other string content.

```csharp
public ContentResult SayHello() {
    return Content("Hello World");
}
```

---

### 4. **RedirectResult / RedirectToActionResult**
Redirects the request to another URL or action.

```csharp
public IActionResult RedirectToGoogle() {
    return Redirect("https://www.google.com");
}

public IActionResult GoToAbout() {
    return RedirectToAction("About");
}
```

---

### 5. **FileResult**
Returns files to download or view.

```csharp
public IActionResult DownloadFile() {
    byte[] fileBytes = System.IO.File.ReadAllBytes("file.pdf");
    return File(fileBytes, "application/pdf", "file.pdf");
}
```

---

### 6. **StatusCodeResult**
Returns a specific HTTP status code.

```csharp
public IActionResult NotFoundPage() {
    return NotFound();
}
```

Other status examples:
```csharp
return Ok();          // 200
return BadRequest();  // 400
return Unauthorized();// 401
return Forbid();      // 403
return NotFound();    // 404
return StatusCode(500);
```

---

## ✅ Return Types Summary

| Return Type           | Description                          |
|-----------------------|--------------------------------------|
| `ViewResult`          | Renders and returns a view           |
| `JsonResult`          | Returns JSON data                    |
| `ContentResult`       | Returns plain text or HTML           |
| `RedirectResult`      | Redirects to external URL            |
| `RedirectToActionResult` | Redirects to controller action   |
| `FileResult`          | Returns a downloadable file          |
| `StatusCodeResult`    | Returns an HTTP status code          |

---

## 🧠 Best Practices

- Return `IActionResult` for flexibility.
- Use `ActionResult<T>` in Web APIs for strong typing and model validation.
- Use `Ok()`, `BadRequest()`, etc., for better readability and semantic clarity.

---

## 📌 Example Controller

```csharp
public class DemoController : Controller
{
    public IActionResult Index() => View();

    public IActionResult GetJson() => Json(new { Name = "TourNest", Category = "Travel" });

    public IActionResult Download() => File(System.IO.File.ReadAllBytes("doc.pdf"), "application/pdf", "doc.pdf");

    public IActionResult RedirectToAbout() => RedirectToAction("About");
}
```

---

## 📚 Summary

Action results define how ASP.NET Core MVC responds to HTTP requests. They allow controllers to return views, data, files, redirects, and status codes, enabling a wide range of behaviors based on your app’s needs.
