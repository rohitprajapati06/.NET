# `Hidden` HTML Helper in .NET Core MVC

## 🧾 Introduction

The `Hidden` HTML Helper in ASP.NET Core MVC is used to render an `<input type="hidden">` element. This field stores data that should not be visible or editable by users but needs to be submitted with the form.

---

## 🛠️ Syntax

```csharp
@Html.Hidden(string name)
@Html.Hidden(string name, object value)
@Html.Hidden(string name, object value, object htmlAttributes)
```

For strongly-typed views:

```csharp
@Html.HiddenFor(expression)
@Html.HiddenFor(expression, object htmlAttributes)
```

---

## 📌 Parameters

| Parameter       | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `name`           | The name of the hidden input field.                                         |
| `value`          | The value stored in the hidden field.                                       |
| `htmlAttributes` | Additional HTML attributes (like id, class, etc.).                         |

---

## ✅ Example 1: Basic Hidden Field

```csharp
@Html.Hidden("UserId", 123)
```

**Output HTML:**

```html
<input type="hidden" name="UserId" value="123" />
```

---

## ✅ Example 2: Strongly Typed Hidden Field

Model:

```csharp
public class UserModel
{
    public int Id { get; set; }
}
```

View:

```csharp
@model UserModel
@Html.HiddenFor(m => m.Id)
```

If `Model.Id = 101`, the output will be:

```html
<input type="hidden" name="Id" id="Id" value="101" />
```

---

## ✅ Example 3: With HTML Attributes

```csharp
@Html.Hidden("Token", "abc123", new { id = "TokenField" })
```

**Output HTML:**

```html
<input type="hidden" name="Token" value="abc123" id="TokenField" />
```

---

## 🧠 Notes

- Hidden fields are useful for preserving data like IDs, timestamps, tokens, etc., across requests.
- Do not store sensitive data (like passwords or auth tokens) in hidden fields unless the data is encrypted or properly validated server-side.
- Use `HiddenFor` in strongly-typed views to leverage model binding.

---

## 🏁 Summary

The `Hidden` HTML Helper provides a way to pass data silently through forms in ASP.NET Core MVC. It’s simple, effective, and essential for tracking information that shouldn’t be altered by the user but must be preserved during postbacks or submissions.