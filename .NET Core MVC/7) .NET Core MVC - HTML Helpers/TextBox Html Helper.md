# `TextBox` HTML Helper in .NET Core MVC

## 🧾 Introduction

In ASP.NET Core MVC, HTML Helpers are methods used in Razor views to generate HTML elements. The `TextBox` HTML helper is specifically used to render an `<input type="text">` element for user input. It’s commonly used to bind input fields to model properties.

---

## 🛠️ Syntax

```csharp
@Html.TextBox(string name)
@Html.TextBox(string name, object value)
@Html.TextBox(string name, object value, object htmlAttributes)
```

---

## 📌 Parameters

| Parameter        | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `name`           | The name of the input field. It should match the model property name.       |
| `value`          | The value to populate the textbox with.                                     |
| `htmlAttributes` | An object containing HTML attributes such as class, placeholder, etc.       |

---

## ✅ Example 1: Simple TextBox

```csharp
@Html.TextBox("Username")
```

**Output HTML:**

```html
<input name="Username" type="text" value="" />
```

---

## ✅ Example 2: TextBox Bound to Model

Assume this model:

```csharp
public class User
{
    public string Name { get; set; }
}
```

In your view (strongly typed to `User`):

```csharp
@model User
@Html.TextBox("Name")
```

Or using `TextBoxFor` (preferred):

```csharp
@Html.TextBoxFor(model => model.Name)
```

**Output HTML:**

```html
<input name="Name" type="text" value="ModelValue" />
```

---

## ✅ Example 3: With HTML Attributes

```csharp
@Html.TextBox("Email", null, new { @class = "form-control", placeholder = "Enter email" })
```

**Output HTML:**

```html
<input name="Email" type="text" class="form-control" placeholder="Enter email" />
```

---

## 🎯 When to Use `TextBox` vs `TextBoxFor`

| Helper        | Use When                                                                 |
|---------------|--------------------------------------------------------------------------|
| `TextBox`     | You need more control over the name/value or aren’t using a model.       |
| `TextBoxFor`  | Preferred when working with strongly typed models for compile-time safety.|

---

## 🧠 Notes

- The `TextBox` helper does **not** automatically bind to model metadata.
- `TextBoxFor` should be used when working with strongly-typed views.
- You can pass attributes like `id`, `class`, `style`, `placeholder`, etc.

---

## 🏁 Summary

The `TextBox` HTML Helper in ASP.NET Core MVC helps in rendering basic input fields. It’s flexible and easy to extend with HTML attributes. For strongly-typed models, use `TextBoxFor` to take full advantage of model binding and validation.