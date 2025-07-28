
# Status Results in ASP.NET Core

In ASP.NET Core MVC, status code results are used to return specific HTTP status codes from controller actions. These are useful for API responses, error handling, and conveying operation results.

---

## 📋 Common Status Code Results

| Method                        | Description                              | HTTP Code |
|-------------------------------|------------------------------------------|-----------|
| `Ok()`                        | Success with optional content            | 200       |
| `Created()` / `CreatedAtAction()` | Resource created                       | 201       |
| `NoContent()`                 | Success but no content                   | 204       |
| `BadRequest()`                | Client error                             | 400       |
| `Unauthorized()`              | Authentication required                  | 401       |
| `Forbidden()`                 | Access denied                            | 403       |
| `NotFound()`                  | Resource not found                       | 404       |
| `Conflict()`                  | Resource conflict                        | 409       |
| `StatusCode(int)`            | Custom status code                       | any       |

---

## ✅ Usage Examples

### 1. `Ok()`

```csharp
return Ok(new { message = "Success", data = result });
```

### 2. `CreatedAtAction()`

```csharp
return CreatedAtAction("GetById", new { id = product.Id }, product);
```

### 3. `NoContent()`

```csharp
return NoContent();
```

---

### 4. `BadRequest()`

```csharp
return BadRequest("Invalid request data");
```

With model state:

```csharp
if (!ModelState.IsValid)
    return BadRequest(ModelState);
```

### 5. `Unauthorized()`

```csharp
return Unauthorized();
```

### 6. `Forbidden()`

```csharp
return Forbid();
```

### 7. `NotFound()`

```csharp
return NotFound("Item not found");
```

### 8. `Conflict()`

```csharp
return Conflict("Duplicate entry detected");
```

---

## 🧰 Custom Status Code

```csharp
return StatusCode(418, "I'm a teapot 🫖");
```

---

## 🧪 Returning Results from APIs

When building Web APIs, these result types help clearly express API semantics and improve client interactions.

```csharp
[HttpPost]
public IActionResult CreateItem(ItemModel item)
{
    if (item == null)
        return BadRequest();

    var created = _repo.Create(item);
    return CreatedAtAction(nameof(GetById), new { id = created.Id }, created);
}
```

---

## 📚 Resources

- [Action Results in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types)
- [Status Codes](https://learn.microsoft.com/en-us/aspnet/core/web-api/handle-errors)

---

> ℹ️ Tip: Choose the most semantically correct status result to improve API clarity and client behavior.
