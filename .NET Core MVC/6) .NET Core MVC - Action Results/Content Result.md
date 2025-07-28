
# ContentResult in ASP.NET Core MVC

## What is `ContentResult`?

`ContentResult` is a type of `ActionResult` in ASP.NET Core MVC that is used to return **plain text**, **HTML**, or **other content** (like JSON or XML) as the HTTP response.

---

## Namespace

```csharp
using Microsoft.AspNetCore.Mvc;
```

---

## When to Use

Use `ContentResult` when you want to return simple textual or HTML content from a controller action, without using a Razor view.

---

## Basic Example

```csharp
public IActionResult HelloWorld()
{
    return Content("Hello, world!");
}
```

**Output:**  
Plain text: `Hello, world!`

---

## Specifying Content Type

```csharp
public IActionResult GetHtml()
{
    return Content("<h1>Hello</h1>", "text/html");
}
```

- `"text/plain"` (default)
- `"text/html"`
- `"application/json"` (if using Content for JSON, prefer `JsonResult`)

---

## Full Syntax

```csharp
return new ContentResult
{
    Content = "Welcome!",
    ContentType = "text/plain",
    StatusCode = 200
};
```

---

## Returning JSON as Content

Though it's better to use `JsonResult`, you can use `ContentResult` for raw JSON:

```csharp
public IActionResult JsonByContent()
{
    string json = "{"name":"John", "age":30}";
    return Content(json, "application/json");
}
```

---

## Comparison with Other Results

| Result Type     | Purpose                                |
|------------------|----------------------------------------|
| `ContentResult`  | Return plain text or HTML              |
| `JsonResult`     | Return JSON with automatic serialization |
| `ViewResult`     | Render a Razor View                    |
| `FileResult`     | Return a file                          |
| `RedirectResult` | Redirect to another URL                |

---

## Summary

- `ContentResult` sends a plain string as the HTTP response.
- Can return HTML, plain text, or any content type.
- Set `ContentType` for proper rendering.
- Best used for quick responses or when not using Razor views.

> Use `ContentResult` for simple, content-based responses directly from your controller.
