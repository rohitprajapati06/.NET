
# Redirect Results in ASP.NET Core

In ASP.NET Core MVC, redirect results are used to instruct the browser to make a new request to a different URL. Redirects are useful for navigation, preventing form resubmission, and controlling flow.

---

## 🔁 Types of Redirect Results

| Method                        | Description                                      |
|-------------------------------|--------------------------------------------------|
| `RedirectToAction`            | Redirects to an action method                    |
| `RedirectToRoute`             | Redirects using route name and values            |
| `Redirect`                    | Redirects to a specified URL                     |
| `LocalRedirect`              | Redirects to a local URL (prevents open redirect attacks) |
| `RedirectPermanent`           | Permanent 301 redirect to a URL                  |
| `RedirectToPage` (Razor Pages) | Redirects to a Razor Page                        |

---

## 📘 RedirectToAction

```csharp
return RedirectToAction("Index", "Home");
```

With route values:

```csharp
return RedirectToAction("Details", new { id = 10 });
```

---

## 📘 RedirectToRoute

```csharp
return RedirectToRoute(new { controller = "Home", action = "Index" });
```

With named route:

```csharp
return RedirectToRoute("default", new { id = 1 });
```

---

## 📘 Redirect

```csharp
return Redirect("https://example.com");
```

---

## 📘 LocalRedirect

```csharp
return LocalRedirect("/Home/Index");
```

> ✅ Ensures the redirect is within the same domain for security.

---

## 📘 RedirectPermanent

```csharp
return RedirectPermanent("/Home/About");
```

> 🚨 Used for SEO and when content has moved permanently (301).

---

## 📘 RedirectToPage (Razor Pages)

```csharp
return RedirectToPage("/Index");
```

With parameters:

```csharp
return RedirectToPage("/Product", new { id = 5 });
```

---

## 📚 Resources

- [Redirecting in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/actions#redirecting)
- [Action Results in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/actions#action-results)

---

> 💡 Tip: Use `RedirectToAction` or `RedirectToRoute` for MVC and `RedirectToPage` for Razor Pages to maintain routing consistency.
