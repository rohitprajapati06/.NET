# 📄 Custom HTML Helper in ASP.NET Core MVC

HTML Helpers in ASP.NET Core MVC are methods that return HTML strings, helping generate HTML UI elements dynamically in Razor views. You can also define your own **custom HTML helpers** to encapsulate reusable HTML rendering logic.

---

## 📌 Table of Contents

1. [What is a Custom HTML Helper?](#what-is-a-custom-html-helper)
2. [Types of HTML Helpers](#types-of-html-helpers)
3. [Creating a Custom HTML Helper (Extension Method)](#creating-a-custom-html-helper-extension-method)
4. [Using the Custom HTML Helper in Razor Views](#using-the-custom-html-helper-in-razor-views)
5. [Creating a Tag Helper (Modern Alternative)](#creating-a-tag-helper-modern-alternative)
6. [Conclusion](#conclusion)

---

## 🔹 What is a Custom HTML Helper?

A custom HTML helper is a method that extends the capabilities of built-in helpers like `Html.TextBoxFor()` or `Html.LabelFor()` and allows you to define reusable, strongly-typed components in Razor views.

---

## 🧩 Types of HTML Helpers

1. **Inline HTML Helpers** – Defined directly in a Razor view.
2. **Extension Method Helpers** – Defined as extension methods for `IHtmlHelper`.
3. **Tag Helpers** – A modern alternative introduced in ASP.NET Core.

---

## ✍️ Creating a Custom HTML Helper (Extension Method)

### Step 1: Create a static class and method

```csharp
using Microsoft.AspNetCore.Mvc.Rendering;
using Microsoft.AspNetCore.Html;

public static class CustomHtmlHelpers
{
    public static IHtmlContent CustomLabel(this IHtmlHelper htmlHelper, string text, string forId)
    {
        var label = $"<label for='{forId}' class='custom-label'>{text}</label>";
        return new HtmlString(label);
    }
}
```

> `HtmlString` is used to return safe HTML output.

---

## 📥 Using the Custom HTML Helper in Razor Views

### Step 2: Add `@using` directive in the view or `_ViewImports.cshtml`

```csharp
@using YourAppNamespace.Helpers
```

### Step 3: Use it in a Razor page

```csharp
@Html.CustomLabel("Full Name", "FullName")
```

**Output:**

```html
<label for='FullName' class='custom-label'>Full Name</label>
```

---

## 🛠️ Creating a Tag Helper (Modern Alternative)

### Example: Custom `<email>` tag

```csharp
using Microsoft.AspNetCore.Razor.TagHelpers;

public class EmailTagHelper : TagHelper
{
    public string Address { get; set; }

    public override void Process(TagHelperContext context, TagHelperOutput output)
    {
        output.TagName = "a";
        output.Attributes.SetAttribute("href", $"mailto:{Address}");
        output.Content.SetContent(Address);
    }
}
```

### Use in Razor View

```html
<email address="user@example.com"></email>
```

---

## 🧾 Conclusion

- Use custom HTML helpers to simplify and reuse UI elements.
- Prefer **extension method helpers** for basic HTML components.
- Use **Tag Helpers** for more advanced, component-like behavior.
- Always return `IHtmlContent` to ensure safe rendering.