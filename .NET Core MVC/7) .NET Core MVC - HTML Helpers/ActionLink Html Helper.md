# 📄 `Html.ActionLink` in ASP.NET Core MVC

`Html.ActionLink` is a built-in HTML helper method in ASP.NET Core MVC used to generate anchor (`<a>`) tags that link to controller actions.

---

## 📌 Table of Contents

1. [What is `Html.ActionLink`?](#what-is-htmlactionlink)
2. [Basic Syntax](#basic-syntax)
3. [Examples of `Html.ActionLink`](#examples-of-htmlactionlink)
4. [Passing Route Values](#passing-route-values)
5. [Applying HTML Attributes](#applying-html-attributes)
6. [Strongly Typed Links (using nameof)](#strongly-typed-links-using-nameof)
7. [Conclusion](#conclusion)

---

## 🔹 What is `Html.ActionLink`?

`Html.ActionLink` generates an HTML anchor (`<a>`) element that links to a specified action method. It's helpful in creating navigational links in a strongly-typed and safe manner.

```csharp
@Html.ActionLink("Link Text", "ActionName")
```

---

## 🔸 Basic Syntax

```csharp
Html.ActionLink(string linkText, string actionName)
Html.ActionLink(string linkText, string actionName, string controllerName)
Html.ActionLink(string linkText, string actionName, object routeValues)
Html.ActionLink(string linkText, string actionName, string controllerName, object routeValues, object htmlAttributes)
```

---

## ✅ Examples of `Html.ActionLink`

### 1. Link to Action in Same Controller

```csharp
@Html.ActionLink("Home", "Index")
```

**Output:**

```html
<a href="/Home/Index">Home</a>
```

---

### 2. Link to Action in Another Controller

```csharp
@Html.ActionLink("Contact", "Contact", "Home")
```

**Output:**

```html
<a href="/Home/Contact">Contact</a>
```

---

## 📦 Passing Route Values

You can pass parameters using anonymous objects:

```csharp
@Html.ActionLink("View Order", "Details", "Orders", new { id = 10 }, null)
```

**Output:**

```html
<a href="/Orders/Details/10">View Order</a>
```

---

## 🎨 Applying HTML Attributes

To add CSS classes or other attributes:

```csharp
@Html.ActionLink("Edit", "Edit", new { id = 5 }, new { @class = "btn btn-primary" })
```

**Output:**

```html
<a class="btn btn-primary" href="/Controller/Edit/5">Edit</a>
```

---

## 💡 Strongly Typed Links (Using `nameof`)

Using `nameof` prevents errors during refactoring:

```csharp
@Html.ActionLink("Back to Home", nameof(HomeController.Index), "Home")
```

---

## 🧾 Conclusion

- `Html.ActionLink` helps generate safe and valid links to MVC actions.
- Supports route values and custom HTML attributes.
- Use `nameof()` to make your links more refactor-safe.