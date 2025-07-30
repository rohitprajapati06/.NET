# 📄 `JsonResult` in ASP.NET Core MVC

`JsonResult` is a built-in action result in ASP.NET Core MVC used to return JSON-formatted data from controller actions.

---

## 📌 Table of Contents

1. [What is `JsonResult`?](#what-is-jsonresult)
2. [Basic Syntax](#basic-syntax)
3. [Return JSON from a Controller](#return-json-from-a-controller)
4. [Customizing JSON Output](#customizing-json-output)
5. [Use Case: API Responses](#use-case-api-responses)
6. [Security Considerations](#security-considerations)
7. [Conclusion](#conclusion)

---

## 🔹 What is `JsonResult`?

`JsonResult` is used when you want to return JSON-formatted data from a controller action. It is commonly used in web APIs or when using AJAX calls in web applications.

```csharp
public JsonResult MyAction()
{
    var data = new { Name = "Rohit", Age = 25 };
    return Json(data);
}
```

---

## 🔸 Basic Syntax

```csharp
public JsonResult Json([ActionResultObjectValue] object value);
```

- `value`: The object to serialize to JSON.

---

## ✅ Return JSON from a Controller

### Example 1: Basic Usage

```csharp
public class HomeController : Controller
{
    public JsonResult GetUser()
    {
        var user = new
        {
            Id = 1,
            Name = "Rohit",
            Email = "rohit@example.com"
        };

        return Json(user);
    }
}
```

---

## ⚙️ Customizing JSON Output

You can customize how JSON is returned by modifying `JsonSerializerOptions` in `Startup.cs`.

### Enable camelCase and ignore nulls:

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    options.JsonSerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
    options.JsonSerializerOptions.DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull;
});
```

---

## 🚀 Use Case: API Responses

Use `JsonResult` to return data to front-end JavaScript frameworks (e.g., React, Angular, jQuery AJAX):

### jQuery Example

```js
$.get("/Home/GetUser", function(data) {
    console.log(data.name); // Outputs "Rohit"
});
```

---

## 🔐 Security Considerations

- Always validate and sanitize input to avoid injection attacks.
- Use `JsonResult` only when appropriate; prefer `Ok()` or `ObjectResult` in Web APIs.
- JSON responses should be restricted from being executed as scripts (set `Content-Type` correctly).

---

## ✅ Alternative: `return Ok(object)` (For APIs)

```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    [HttpGet]
    public IActionResult GetUser()
    {
        var user = new { Id = 1, Name = "Rohit" };
        return Ok(user); // Also returns JSON
    }
}
```

---

## 🧾 Conclusion

- `JsonResult` is ideal for returning JSON from MVC controllers.
- Customize globally with `JsonOptions`.
- Use carefully with security and performance in mind.