# HTML ListBox Helper in ASP.NET Core MVC

## ✅ 1. Syntax

```csharp
@Html.ListBoxFor(model => model.SelectedItems, Model.ItemList)
```

## ✅ 2. Example

### Model

```csharp
public class ListBoxViewModel
{
    public List<string> SelectedItems { get; set; } = new List<string>();

    public List<SelectListItem> ItemList { get; set; } = new List<SelectListItem>
    {
        new SelectListItem { Text = "Item 1", Value = "1" },
        new SelectListItem { Text = "Item 2", Value = "2" },
        new SelectListItem { Text = "Item 3", Value = "3" }
    };
}
```

### Controller

```csharp
public IActionResult MultiSelect()
{
    var model = new ListBoxViewModel();
    return View(model);
}

[HttpPost]
public IActionResult MultiSelect(ListBoxViewModel model)
{
    if (ModelState.IsValid)
    {
        // Access model.SelectedItems
    }
    return View(model);
}
```

### View (`MultiSelect.cshtml`)

```html
@model YourApp.Models.ListBoxViewModel

<form asp-action="MultiSelect" method="post">
    <div>
        @Html.ListBoxFor(m => m.SelectedItems, Model.ItemList, new { @class = "form-control", size = 5 })
    </div>
    <button type="submit">Submit</button>
</form>
```

## ✅ 3. Notes

- `ListBoxFor` binds to a collection of selected values.
- Use the `size` attribute to set the number of visible rows.
- Multiple selections are allowed by default.
- Best used when you want to allow users to select multiple options without holding Ctrl (on mobile or touch interfaces).