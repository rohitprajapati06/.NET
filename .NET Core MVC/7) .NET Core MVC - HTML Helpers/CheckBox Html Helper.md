# HTML CheckBox Helper in ASP.NET Core MVC

## ✅ 1. Syntax

```csharp
@Html.CheckBoxFor(model => model.PropertyName)
```

## ✅ 2. Example

### Model

```csharp
public class PreferencesViewModel
{
    public bool ReceiveNewsletter { get; set; }
}
```

### Controller

```csharp
public IActionResult Preferences()
{
    var model = new PreferencesViewModel();
    return View(model);
}

[HttpPost]
public IActionResult Preferences(PreferencesViewModel model)
{
    if (ModelState.IsValid)
    {
        // Process form
    }
    return View(model);
}
```

### View (`Preferences.cshtml`)

```html
@model YourApp.Models.PreferencesViewModel

<form asp-action="Preferences" method="post">
    <div>
        @Html.LabelFor(m => m.ReceiveNewsletter)
        @Html.CheckBoxFor(m => m.ReceiveNewsletter)
    </div>

    <button type="submit">Submit</button>
</form>
```

## ✅ 3. Manual Checkbox with Attributes

```csharp
@Html.CheckBox("ReceiveNewsletter", true, new { @class = "form-check-input", id = "newsletterCheckbox" })
```