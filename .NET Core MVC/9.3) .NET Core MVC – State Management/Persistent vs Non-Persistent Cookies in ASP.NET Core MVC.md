# Persistent vs Non-Persistent Cookies in ASP.NET Core MVC

Cookies can be classified based on their lifetime into:

1. **Persistent Cookies**
2. **Non-Persistent (Session) Cookies**

---

# Non-Persistent Cookies (Session Cookies)

A non-persistent cookie exists only for the duration of the browser session.

### Characteristics

* Stored in browser memory.
* Removed automatically when the browser closes.
* No expiration date is set.
* Commonly used for temporary user data.

### Example

```csharp
public IActionResult SetSessionCookie()
{
    Response.Cookies.Append(
        "UserName",
        "Rohit");

    return Content("Session Cookie Created");
}
```

### Browser Behavior

```text
User Opens Browser
        ↓
Cookie Created
        ↓
User Navigates Website
        ↓
Browser Closes
        ↓
Cookie Deleted Automatically
```

### Common Uses

* Temporary authentication
* Shopping cart during a single session
* Temporary user preferences
* Tracking user activity for the current session

---

# Persistent Cookies

A persistent cookie remains stored on the user's device until its expiration date or until it is manually deleted.

### Characteristics

* Stored on disk by the browser.
* Has an expiration date.
* Survives browser restarts.
* Available across multiple sessions.

### Example

```csharp
public IActionResult SetPersistentCookie()
{
    var options = new CookieOptions
    {
        Expires = DateTime.UtcNow.AddDays(30)
    };

    Response.Cookies.Append(
        "UserName",
        "Rohit",
        options);

    return Content("Persistent Cookie Created");
}
```

### Browser Behavior

```text
User Opens Browser
        ↓
Cookie Created
        ↓
Browser Closed
        ↓
Browser Reopened
        ↓
Cookie Still Exists
        ↓
Expires After 30 Days
```

### Common Uses

* Remember Me functionality
* Language preference
* Theme preference
* User personalization settings
* Auto-login features

---

# Example: Remember Me Feature

When a user selects **Remember Me** during login:

```csharp
await _signInManager.PasswordSignInAsync(
    model.Email,
    model.Password,
    model.RememberMe,
    false);
```

### If RememberMe = false

ASP.NET Core creates a **non-persistent cookie**.

```text
Close Browser
      ↓
User Logged Out
```

### If RememberMe = true

ASP.NET Core creates a **persistent cookie**.

```text
Close Browser
      ↓
Open Browser Again
      ↓
Still Logged In
```

---

# Creating a Persistent Authentication Cookie

```csharp
builder.Services
    .AddAuthentication("CookieAuth")
    .AddCookie("CookieAuth", options =>
    {
        options.ExpireTimeSpan = TimeSpan.FromDays(30);
        options.SlidingExpiration = true;
    });
```

### Login

```csharp
await HttpContext.SignInAsync(
    "CookieAuth",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

`IsPersistent = true` tells ASP.NET Core to create a persistent authentication cookie.

---

# Creating a Non-Persistent Authentication Cookie

```csharp
await HttpContext.SignInAsync(
    "CookieAuth",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = false
    });
```

or simply:

```csharp
await HttpContext.SignInAsync(
    "CookieAuth",
    principal);
```

The cookie will be removed when the browser session ends.

---

# Comparison Table

| Feature                  | Persistent Cookie        | Non-Persistent Cookie     |
| ------------------------ | ------------------------ | ------------------------- |
| Storage                  | Disk                     | Browser Memory            |
| Expiration Date          | Yes                      | No                        |
| Survives Browser Restart | Yes                      | No                        |
| Deleted On Browser Close | No                       | Yes                       |
| Suitable For             | Remember Me, Preferences | Temporary Session Data    |
| Security                 | Lower (longer lifetime)  | Higher (shorter lifetime) |

---

# Checking Cookie Expiration

### Persistent Cookie

```csharp
var options = new CookieOptions
{
    Expires = DateTime.UtcNow.AddDays(7)
};

Response.Cookies.Append(
    "Theme",
    "Dark",
    options);
```

The browser stores:

```text
Theme=Dark
Expires=19-Jun-2026
```

### Session Cookie

```csharp
Response.Cookies.Append(
    "Theme",
    "Dark");
```

The browser stores:

```text
Theme=Dark
Session Cookie
```

No expiration date is present.

---

# Security Considerations

### Persistent Cookies

Pros:

* Better user experience
* Supports auto-login
* Stores preferences

Cons:

* Can be stolen if a device is compromised
* Remains available for a long period

Use:

```csharp
HttpOnly = true
Secure = true
SameSite = SameSiteMode.Strict
```

---

### Non-Persistent Cookies

Pros:

* Automatically removed after browser closure
* Lower security risk
* Suitable for sensitive applications

Cons:

* User must log in again after closing the browser

---

# Interview Questions

### What is a Persistent Cookie?

A cookie that contains an expiration date and remains stored on the user's device even after the browser is closed.

---

### What is a Non-Persistent Cookie?

A cookie that exists only during the current browser session and is deleted when the browser closes.

---

### How do you create a Persistent Cookie in ASP.NET Core MVC?

```csharp
Response.Cookies.Append(
    "UserName",
    "Rohit",
    new CookieOptions
    {
        Expires = DateTime.UtcNow.AddDays(30)
    });
```

---

### Which cookie type is used for "Remember Me"?

**Persistent Cookies**.

---

### Which cookie type is more secure?

Generally, **Non-Persistent Cookies** are more secure because they are automatically removed when the browser session ends.

---

# Real-World Example

In your **Smart Healthcare System**:

### Non-Persistent Cookie

Doctor logs into the admin portal:

```text
Browser Closed
      ↓
Login Session Ends
```

Useful for hospital computers.

---

### Persistent Cookie

Patient selects:

```text
☑ Remember Me
```

ASP.NET Core creates a persistent authentication cookie:

```text
Close Browser
      ↓
Open After 5 Days
      ↓
Still Logged In
```

This provides a better user experience while maintaining security through expiration dates and secure cookie settings.
