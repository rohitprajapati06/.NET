# 📄 `ObjectResult` in ASP.NET Core MVC

`ObjectResult` is an action result that wraps an object and performs content negotiation to format the response (usually as JSON or XML) based on the request's `Accept` header.

---

## 📌 Table of Contents

1. [What is `ObjectResult`?](#what-is-objectresult)
2. [Basic Syntax](#basic-syntax)
3. [Return Object from a Controller](#return-object-from-a-controller)
4. [Content Negotiation](#content-negotiation)
5. [Use Case: API Responses](#use-case-api-responses)
6. [ObjectResult vs JsonResult](#objectresult-vs-jsonresult)
7. [Conclusion](#conclusion)

---

## 🔹 What is `ObjectResult`?

`ObjectResult` is a flexible way to return data from Web API controllers in ASP.NET Core. It supports content negotiation and returns the best format based on the client's request.

```csharp
return new ObjectResult(myObject);
```

---

## 🔸 Basic Syntax

```csharp
public ObjectResult(object value);
```

- `value`: The object to wrap and return to the client.

---

## ✅ Return Object from a Controller

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetProduct(int id)
    {
        var product = new { Id = id, Name = "Laptop", Price = 50000 };
        return new ObjectResult(product);
    }
}
```

---

## 🌐 Content Negotiation

`ObjectResult` supports automatic content negotiation. Depending on the `Accept` header, it can return:

- `application/json`
- `application/xml` (if configured)
- `text/plain`, etc.

```http
GET /api/products/1
Accept: application/json
```

The response will be serialized as JSON if that is the best match.

---

## 🚀 Use Case: API Responses

`ObjectResult` is perfect for building RESTful APIs where response format may vary depending on the client.

You can also use helper methods like `Ok(object)` or `BadRequest(object)` which internally return `ObjectResult`.

```csharp
return Ok(new { message = "Success" }); // returns ObjectResult
```

---

## 🔄 ObjectResult vs JsonResult

| Feature            | ObjectResult                        | JsonResult                      |
|--------------------|--------------------------------------|----------------------------------|
| Content Negotiation| ✅ Yes                               | ❌ No (always returns JSON)      |
| Custom Formatting  | ✅ Based on `Accept` header           | ❌ Fixed to JSON                 |
| Use in API         | ✅ Recommended                       | ⚠️ Limited use                  |

---

## 🧾 Conclusion

- Use `ObjectResult` for flexible and content-negotiated responses.
- It is recommended in Web API scenarios.
- For MVC (non-API) actions or fixed JSON response, `JsonResult` can be used.