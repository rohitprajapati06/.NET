# `DropDownList` HTML Helper in .NET Core MVC

## 🧾 Introduction

In ASP.NET Core MVC, the `DropDownList` HTML helper is used to render a `<select>` HTML element. It helps in creating dropdown lists by binding them to model properties or manually defining items.

---

## 🛠️ Syntax

```csharp
@Html.DropDownList(string name, IEnumerable<SelectListItem> selectList)
@Html.DropDownList(string name, IEnumerable<SelectListItem> selectList, string optionLabel)
@Html.DropDownList(string name, IEnumerable<SelectListItem> selectList, object htmlAttributes)
@Html.DropDownList(string name, IEnumerable<SelectListItem> selectList, string optionLabel, object htmlAttributes)
```

---

## 📌 Parameters

| Parameter       | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `name`           | Name of the dropdown field (should match model property name).              |
| `selectList`     | Collection of `SelectListItem` objects.                                     |
| `optionLabel`    | Text for a default item (e.g., "-- Select --").                             |
| `htmlAttributes` | HTML attributes like `class`, `id`, `style`, etc.                           |

---

## ✅ Example 1: Simple Dropdown

```csharp
@Html.DropDownList("City", new List<SelectListItem>
{
    new SelectListItem { Text = "Delhi", Value = "1" },
    new SelectListItem { Text = "Mumbai", Value = "2" },
    new SelectListItem { Text = "Bangalore", Value = "3" }
})
```

**Output HTML:**

```html
<select name="City">
  <option value="1">Delhi</option>
  <option value="2">Mumbai</option>
  <option value="3">Bangalore</option>
</select>
```

---

## ✅ Example 2: Dropdown with Default Option

```csharp
@Html.DropDownList("City", new List<SelectListItem>
{
    new SelectListItem { Text = "Delhi", Value = "1" },
    new SelectListItem { Text = "Mumbai", Value = "2" },
    new SelectListItem { Text = "Bangalore", Value = "3" }
}, "-- Select City --")
```

**Output HTML:**

```html
<select name="City">
  <option value="">-- Select City --</option>
  <option value="1">Delhi</option>
  <option value="2">Mumbai</option>
  <option value="3">Bangalore</option>
</select>
```

---

## ✅ Example 3: Strongly Typed Dropdown (`DropDownListFor`)

Model:

```csharp
public class UserModel
{
    public int SelectedCityId { get; set; }
    public List<SelectListItem> Cities { get; set; }
}
```

View:

```csharp
@model UserModel
@Html.DropDownListFor(m => m.SelectedCityId, Model.Cities, "-- Select City --")
```

---

## 🧠 Notes

- `DropDownList` is ideal for loosely typed or non-model-bound forms.
- Use `DropDownListFor` for strongly typed views to get model binding and validation.
- The selected item is matched by comparing the `Value` with the model value.

---

## 🏁 Summary

The `DropDownList` HTML helper in ASP.NET Core MVC helps generate dropdowns using lists of items. It supports default labels, custom HTML attributes, and is best used with models through `DropDownListFor` for binding and validation.