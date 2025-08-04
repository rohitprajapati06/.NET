# `Password` HTML Helper in .NET Core MVC

## 🧾 Introduction

The `Password` HTML Helper in ASP.NET Core MVC is used to render an `<input type="password">` element. This input hides characters as they are typed, making it suitable for sensitive data like passwords.

---

## 🛠️ Syntax

```csharp
@Html.Password(string name)
@Html.Password(string name, object value)
@Html.Password(string name, object value, object htmlAttributes)
```

For strongly-typed views, use:

```csharp
@Html.PasswordFor(expression)
@Html.PasswordFor(expression, object htmlAttributes)
```

---

## 📌 Parameters

| Parameter       | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `name`           | The name attribute of the input field.                                      |
| `value`          | The default value for the password input (generally left blank for security). |
| `htmlAttributes` | Additional HTML attributes for the element.                                |

---

## ✅ Example 1: Basic Password Field

```csharp
@Html.Password("UserPassword")
```

**Output HTML:**

```html
<input type="password" name="UserPassword" />
```

---

## ✅ Example 2: With HTML Attributes

```csharp
@Html.Password("UserPassword", null, new { @class = "form-control", placeholder = "Enter password" })
```

**Output HTML:**

```html
<input type="password" name="UserPassword" class="form-control" placeholder="Enter password" />
```

---

## ✅ Example 3: Strongly Typed Password Field

Model:

```csharp
public class LoginModel
{
    public string Password { get; set; }
}
```

View:

```csharp
@model LoginModel
@Html.PasswordFor(m => m.Password, new { @class = "form-control", placeholder = "Enter your password" })
```

**Output HTML:**
```html
<input type="password" class="form-control" placeholder="Enter your password" name="Password" id="Password" />
```

---

## 🧠 Notes

- Password fields are not pre-filled for security reasons.
- Avoid setting a default value unless absolutely necessary.
- Use `PasswordFor` in strongly-typed views to take advantage of model binding and validation.

---

## 🏁 Summary

The `Password` HTML Helper is useful for creating secure password input fields. In strongly-typed views, prefer `PasswordFor` to bind the input to model properties and apply validation rules automatically.