# 📄 `EmptyResult` in ASP.NET Core MVC

`EmptyResult` is an action result in ASP.NET Core MVC that represents a response with no content. It is used when you don't want to return anything to the client.

---

## 📌 Table of Contents

1. [What is `EmptyResult`?](#what-is-emptyresult)
2. [Basic Syntax](#basic-syntax)
3. [Return Empty Result from a Controller](#return-empty-result-from-a-controller)
4. [When to Use `EmptyResult`](#when-to-use-emptyresult)
5. [Alternative Approaches](#alternative-approaches)
6. [Conclusion](#conclusion)

---

## 🔹 What is `EmptyResult`?

`EmptyResult` is used when an action method has nothing to return. It results in a 204 No Content response.

```csharp
return new EmptyResult();
```

---

## 🔸 Basic Syntax

```csharp
public class EmptyResult : ActionResult
```

---

## ✅ Return Empty Result from a Controller

### Example

```csharp
public class SampleController : Controller
{
    public IActionResult DoNothing()
    {
        // Perform some logic here

        return new EmptyResult();
    }
}
```

---

## 📘 When to Use `EmptyResult`

- When you want to return **no content** but avoid returning `null`.
- To explicitly signal that no result should be returned.
- Useful in scenarios like background logging, signal-only actions, or AJAX calls where the result doesn't matter.

---

## 🔄 Alternative Approaches

### Use `NoContent()` (Preferred in APIs)

```csharp
return NoContent(); // returns 204 No Content
```

### Return `null` (not recommended)

Returning `null` can lead to unexpected results or exceptions. `EmptyResult` is safer and more explicit.

---

## 🧾 Conclusion

- `EmptyResult` is a simple way to return no content from an MVC action.
- It’s more explicit and safer than returning `null`.
- For Web APIs, prefer using `NoContent()` which also returns 204 status code.