# Cookies in ASP.NET Core MVC

Cookies are small pieces of data stored in the user's browser. ASP.NET Core MVC uses cookies to maintain user-specific information such as authentication, preferences, shopping carts, and session identifiers.

---

# Why Use Cookies?

Cookies help in:

* Remembering user login information
* Storing user preferences (theme, language, etc.)
* Tracking user activity
* Maintaining session state
* Implementing "Remember Me" functionality

---

# Types of Cookies

## 1. Session Cookies

* Stored temporarily in browser memory.
* Deleted when the browser closes.

```csharp
Response.Cookies.Append("UserName", "Rohit");
```

---

## 2. Persistent Cookies

* Remain on the user's device until expiration.

```csharp
var options = new CookieOptions
{
    Expires = DateTime.Now.AddDays(30)
};

Response.Cookies.Append("UserName", "Rohit", options);
```

---

# Writing Cookies

Use `Response.Cookies.Append()`.

## Example

### Controller

```csharp
public IActionResult SetCookie()
{
    Response.Cookies.Append(
        "UserName",
        "Rohit");

    return Content("Cookie Created");
}
```

---

# Reading Cookies

Use `Request.Cookies`.

## Example

```csharp
public IActionResult GetCookie()
{
    string userName = Request.Cookies["UserName"];

    return Content($"Cookie Value: {userName}");
}
```

---

# Updating Cookies

Simply write the cookie again with the same key.

```csharp
public IActionResult UpdateCookie()
{
    Response.Cookies.Append(
        "UserName",
        "Updated Rohit");

    return Content("Cookie Updated");
}
```

---

# Deleting Cookies

Use `Delete()`.

```csharp
public IActionResult DeleteCookie()
{
    Response.Cookies.Delete("UserName");

    return Content("Cookie Deleted");
}
```

---

# CookieOptions

Cookie behavior can be customized using `CookieOptions`.

```csharp
var options = new CookieOptions
{
    Expires = DateTime.Now.AddDays(7),
    HttpOnly = true,
    Secure = true,
    SameSite = SameSiteMode.Strict
};

Response.Cookies.Append(
    "UserName",
    "Rohit",
    options);
```

---

# Important Cookie Options

| Property    | Description                  |
| ----------- | ---------------------------- |
| Expires     | Cookie expiration date       |
| HttpOnly    | Prevent JavaScript access    |
| Secure      | Send only over HTTPS         |
| SameSite    | Controls cross-site requests |
| Domain      | Restrict cookie domain       |
| Path        | Restrict cookie path         |
| IsEssential | Bypass consent requirement   |

---

# Secure Cookie Example

```csharp
var options = new CookieOptions
{
    HttpOnly = true,
    Secure = true,
    SameSite = SameSiteMode.Strict,
    Expires = DateTime.Now.AddDays(7)
};

Response.Cookies.Append(
    "AuthToken",
    "abc123",
    options);
```

---

# Authentication Cookies

ASP.NET Core Identity uses cookies internally to maintain authentication.

### Login Flow

```text
User Login
      ↓
Identity Validates User
      ↓
Authentication Cookie Created
      ↓
Cookie Stored in Browser
      ↓
Browser Sends Cookie on Every Request
      ↓
User Remains Logged In
```

---

## Example

```csharp
await _signInManager.PasswordSignInAsync(
    model.Email,
    model.Password,
    model.RememberMe,
    false);
```

ASP.NET Core automatically creates an authentication cookie after successful login.

---

# Cookie Consent

For GDPR and privacy compliance, you may ask users for cookie consent.

### Program.cs

```csharp
builder.Services.Configure<CookiePolicyOptions>(options =>
{
    options.CheckConsentNeeded = context => true;
    options.MinimumSameSitePolicy = SameSiteMode.Lax;
});
```

```csharp
app.UseCookiePolicy();
```

---

# Cookie Authentication Configuration

```csharp
builder.Services
    .AddAuthentication("CookieAuth")
    .AddCookie("CookieAuth", options =>
    {
        options.LoginPath = "/Account/Login";
        options.AccessDeniedPath = "/Account/AccessDenied";
        options.ExpireTimeSpan = TimeSpan.FromDays(7);
    });
```

---

# Creating Custom Authentication Cookie

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, "Rohit"),
    new Claim(ClaimTypes.Email, "rohit@gmail.com")
};

var identity = new ClaimsIdentity(
    claims,
    "CookieAuth");

var principal = new ClaimsPrincipal(identity);

await HttpContext.SignInAsync(
    "CookieAuth",
    principal);
```

---

# Logout (Remove Authentication Cookie)

```csharp
await HttpContext.SignOutAsync("CookieAuth");
```

---

# Cookies vs Session

| Feature                | Cookies      | Session         |
| ---------------------- | ------------ | --------------- |
| Stored In              | Browser      | Server          |
| Size Limit             | ~4 KB        | Larger          |
| Security               | Less Secure  | More Secure     |
| Lifetime               | Configurable | Session Timeout |
| Performance            | Faster       | Slightly Slower |
| Requires Server Memory | No           | Yes             |

---

# Best Practices

### ✓ Store Small Data Only

Good:

```text
User Preference
Theme
Language
Session Id
```

Avoid:

```text
Large Objects
Images
Files
```

---

### ✓ Use HttpOnly

```csharp
HttpOnly = true
```

Prevents JavaScript from accessing cookies.

---

### ✓ Use Secure Cookies

```csharp
Secure = true
```

Ensures cookies are sent only over HTTPS.

---

### ✓ Use SameSite

```csharp
SameSite = SameSiteMode.Strict
```

Helps protect against CSRF attacks.

---

### ✓ Never Store Sensitive Data

Do NOT store:

* Passwords
* Credit Card Numbers
* JWT Secrets
* Personal Sensitive Information

---

# Common Interview Questions

### 1. What are cookies in ASP.NET Core MVC?

Cookies are small text files stored in the user's browser to maintain state and user-specific information across requests.

---

### 2. How do you create a cookie?

```csharp
Response.Cookies.Append(
    "UserName",
    "Rohit");
```

---

### 3. How do you read a cookie?

```csharp
var value = Request.Cookies["UserName"];
```

---

### 4. What is HttpOnly?

A security setting that prevents client-side JavaScript from accessing the cookie.

---

### 5. What is the difference between Session and Cookies?

Cookies are stored on the client browser, while Session data is stored on the server.

---

### 6. Why is SameSite important?

It helps prevent Cross-Site Request Forgery (CSRF) attacks by controlling when cookies are sent with cross-site requests.

---

# Real-World Usage in Your Projects

For your **La Caffeine Restaurant App** and **Smart Healthcare System**, cookies are commonly used for:

* ASP.NET Core Identity authentication
* Remember Me functionality
* User language preference
* Theme preference (Dark/Light Mode)
* Shopping cart/session tracking
* Storing consent preferences
* Maintaining logged-in user state across requests

In modern ASP.NET Core applications, cookies are most frequently used through **Identity Authentication**, while larger user data is typically stored in the database and referenced through user claims or session IDs.
