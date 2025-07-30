# HTML Editor Helper in ASP.NET Core MVC

## ✅ 1. Syntax

```csharp
@Html.EditorFor(model => model.PropertyName)
```

## ✅ 2. Example

### Model

```csharp
public class EditorViewModel
{
    public string Description { get; set; }
}
```

### Controller

```csharp
public IActionResult Edit()
{
    var model = new EditorViewModel
    {
        Description = "Initial text in the editor"
    };
    return View(model);
}

[HttpPost]
public IActionResult Edit(EditorViewModel model)
{
    if (ModelState.IsValid)
    {
        // Process the submitted description
    }
    return View(model);
}
```

### View (`Edit.cshtml`)

```html
@model YourApp.Models.EditorViewModel

<form asp-action="Edit" method="post">
    <div>
        @Html.LabelFor(m => m.Description)
        @Html.EditorFor(m => m.Description)
    </div>
    <button type="submit">Save</button>
</form>
```

## ✅ 3. Notes

- `EditorFor` is a strongly-typed helper that renders an appropriate input field based on the property type.
- For strings, it defaults to a multi-line `<textarea>`.
- Can be customized using [Editor Templates](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/overview#editor-templates).
- You can style the rendered HTML using CSS or customize it further via custom templates in `/Views/Shared/EditorTemplates/`.

## ✅ 4. Customizing Editor

To customize the editor for `Description`, add a partial view at:

**`/Views/Shared/EditorTemplates/String.cshtml`**

```html
@model string
<textarea class="form-control custom-editor" rows="5" name="@ViewData.TemplateInfo.GetFullHtmlFieldName("")">
    @Model
</textarea>
```

This will apply to all `string` types using `EditorFor` unless overridden with `[UIHint]` attributes.