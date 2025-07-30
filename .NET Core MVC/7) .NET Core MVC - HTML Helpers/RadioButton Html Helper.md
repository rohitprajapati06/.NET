# `RadioButton` HTML Helper in .NET Core MVC

## 🧾 Introduction

The `RadioButton` HTML Helper in ASP.NET Core MVC is used to render an HTML `<input type="radio">` element. It allows users to select a single option from a group. The helper can be used in both non-strongly typed and strongly-typed views.

---

## 🛠️ Syntax

```csharp
@Html.RadioButton(string name, object value)
@Html.RadioButton(string name, object value, bool isChecked)
@Html.RadioButton(string name, object value, object htmlAttributes)
```

---

## 📌 Parameters

| Parameter       | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `name`           | The name attribute of the radio button (should match model property name). |
| `value`          | The value associated with the radio button.                                |
| `isChecked`      | Specifies whether the radio button is initially selected.                  |
| `htmlAttributes` | An object containing additional HTML attributes.                           |

---

## ✅ Example 1: Basic Radio Buttons

```csharp
@Html.RadioButton("Gender", "Male") Male
@Html.RadioButton("Gender", "Female") Female
```

**Output HTML:**

```html
<input type="radio" name="Gender" value="Male" /> Male
<input type="radio" name="Gender" value="Female" /> Female
```

---

## ✅ Example 2: With Model Binding (Strongly Typed View)

Model:

```csharp
public class UserModel
{
    public string Gender { get; set; }
}
```

View:

```csharp
@model UserModel
@Html.RadioButtonFor(m => m.Gender, "Male") Male
@Html.RadioButtonFor(m => m.Gender, "Female") Female
```

If `Gender = "Male"` in the model, the "Male" button will be selected.

---

## ✅ Example 3: With HTML Attributes

```csharp
@Html.RadioButton("Gender", "Male", new { @class = "form-check-input" }) Male
```

**Output HTML:**

```html
<input type="radio" name="Gender" value="Male" class="form-check-input" /> Male
```

---

## 🎯 Notes

- All radio buttons in a group should share the same `name` so only one can be selected at a time.
- `RadioButtonFor` is preferred for strongly-typed views for model binding and validation.
- Use conditionals to determine which radio button should be checked based on model value.

---

## 🏁 Summary

The `RadioButton` HTML Helper in ASP.NET Core MVC is useful for rendering a group of radio buttons for a model property or form field. For best practices, use `RadioButtonFor` in strongly-typed views to enable proper model binding and form submission.