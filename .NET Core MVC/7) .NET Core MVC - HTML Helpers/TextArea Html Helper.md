
# 📝 Html.TextAreaFor in ASP.NET Core MVC

The `Html.TextAreaFor()` HTML Helper is used to generate a `<textarea>` element in Razor views that is bound to a model property. It's ideal for multi-line text inputs such as comments, descriptions, and notes.

---

## ✅ Syntax

### Strongly Typed

```csharp
@Html.TextAreaFor(model => model.Description)
```

### Non-Strongly Typed

```csharp
@Html.TextArea("Description", "Initial value", new { @class = "form-control", rows = 4, cols = 50 })
```

---

## 🔧 Parameters

```csharp
Html.TextAreaFor(
    Expression<Func<TModel, TProperty>> expression,
    object htmlAttributes
)
```

### Example:

```csharp
@Html.TextAreaFor(model => model.Comments, new { @class = "form-control", rows = 5, cols = 50 })
```

### Renders as:

```html
<textarea class="form-control" cols="50" id="Comments" name="Comments" rows="5">
<!-- model.Comments value here -->
</textarea>
```

---

## 🧾 Full Example

### Model

```csharp
public class Feedback
{
    public string Comments { get; set; }
}
```

### View (`.cshtml`)

```csharp
@model MyApp.Models.Feedback

<form asp-action="SubmitFeedback" method="post">
    <div>
        <label asp-for="Comments"></label>
        @Html.TextAreaFor(m => m.Comments, new { @class = "form-control", rows = 6 })
        @Html.ValidationMessageFor(m => m.Comments)
    </div>
    <button type="submit">Submit</button>
</form>
```

---

## 🎯 When to Use

Use `TextAreaFor()` when you need:

- Multi-line user input
- Comments section
- Blog content editor
- Long descriptions in forms

---

## 📘 Tip

Always pair `TextAreaFor()` with `ValidationMessageFor()` to display validation feedback for the user.
