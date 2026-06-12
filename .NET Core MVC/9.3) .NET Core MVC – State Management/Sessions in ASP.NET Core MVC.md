# Sessions in ASP.NET Core MVC

A **Session** is a server-side storage mechanism used to store user-specific data across multiple HTTP requests.

Since HTTP is stateless, sessions help maintain user information while the user navigates through the application.

---

# Why Do We Need Sessions?

HTTP treats every request as independent.

Without sessions:

```text
Request 1 → Login
Request 2 → View Profile
Request 3 → Add Item to Cart

Server does not remember previous requests.
```

With sessions:

```text
Request 1 → Login
      ↓
Session Created

Request 2 → View Profile
      ↓
Session Data Available

Request 3 → Add Item to Cart
      ↓
Session Data Available
```

---

# How Sessions Work

```text
User Sends Request
        ↓
Server Creates Session
        ↓
Unique Session ID Generated
        ↓
Session ID Stored in Cookie
        ↓
Browser Sends Session ID
        ↓
Server Retrieves Session Data
```

### Important

The actual session data is stored on the **server**.

The browser only stores the **Session ID**.

---

# Session Architecture

```text
Browser
   │
   │ Session Cookie
   ▼
ASP.NET Core Application
   │
   │ Session ID
   ▼
Server Memory / Distributed Cache
   │
   ▼
Session Data
```

---

# Configure Session in ASP.NET Core MVC

## Step 1: Register Distributed Memory Cache

### Program.cs

```csharp
builder.Services.AddDistributedMemoryCache();
```

Sessions require a cache provider.

---

## Step 2: Register Session Services

```csharp
builder.Services.AddSession(options =>
{
    options.IdleTimeout = TimeSpan.FromMinutes(30);
    options.Cookie.HttpOnly = true;
    options.Cookie.IsEssential = true;
});
```

---

## Step 3: Enable Session Middleware

```csharp
var app = builder.Build();

app.UseSession();
```

### Correct Middleware Order

```csharp
app.UseRouting();

app.UseAuthentication();
app.UseAuthorization();

app.UseSession();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

---

# Writing Data to Session

Use:

```csharp
HttpContext.Session.SetString();
HttpContext.Session.SetInt32();
```

### Example

```csharp
public IActionResult SaveData()
{
    HttpContext.Session.SetString(
        "UserName",
        "Rohit");

    HttpContext.Session.SetInt32(
        "UserId",
        101);

    return Content("Data Saved");
}
```

---

# Reading Data from Session

### Example

```csharp
public IActionResult GetData()
{
    string userName =
        HttpContext.Session.GetString("UserName");

    int? userId =
        HttpContext.Session.GetInt32("UserId");

    return Content(
        $"{userName} - {userId}");
}
```

---

# Removing Specific Session Data

```csharp
HttpContext.Session.Remove("UserName");
```

### Example

```csharp
public IActionResult RemoveData()
{
    HttpContext.Session.Remove("UserName");

    return Content("Session Removed");
}
```

---

# Clearing Entire Session

```csharp
HttpContext.Session.Clear();
```

### Example

```csharp
public IActionResult ClearSession()
{
    HttpContext.Session.Clear();

    return Content("All Session Data Removed");
}
```

---

# Storing Complex Objects in Session

Session supports only:

* String
* Int32
* Byte Array

For complex objects, serialize them.

---

## Example Model

```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

---

## Save Object

```csharp
var user = new User
{
    Id = 1,
    Name = "Rohit"
};

string json =
    JsonSerializer.Serialize(user);

HttpContext.Session.SetString(
    "UserData",
    json);
```

---

## Read Object

```csharp
string json =
    HttpContext.Session.GetString("UserData");

User user =
    JsonSerializer.Deserialize<User>(json);
```

---

# Session Timeout

```csharp
builder.Services.AddSession(options =>
{
    options.IdleTimeout =
        TimeSpan.FromMinutes(20);
});
```

### Meaning

```text
User Active
     ↓
Session Exists

No Activity For 20 Minutes
     ↓
Session Expires
```

---

# Session Cookie Settings

```csharp
builder.Services.AddSession(options =>
{
    options.Cookie.Name = ".MyApp.Session";

    options.Cookie.HttpOnly = true;

    options.Cookie.SecurePolicy =
        CookieSecurePolicy.Always;

    options.Cookie.SameSite =
        SameSiteMode.Strict;
});
```

---

# Distributed Session (Production)

For multiple servers, avoid in-memory sessions.

Instead use:

* SQL Server
* Redis
* Distributed Cache

### Redis Example

```csharp
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration =
        "localhost:6379";
});

builder.Services.AddSession();
```

---

# Session vs Cookies

| Feature                | Session       | Cookie           |
| ---------------------- | ------------- | ---------------- |
| Storage                | Server        | Browser          |
| Size Limit             | Larger        | ~4 KB            |
| Security               | More Secure   | Less Secure      |
| Lifetime               | Timeout Based | Expiration Based |
| Server Memory Required | Yes           | No               |
| Suitable For           | User State    | Preferences      |

---

# Session vs TempData vs ViewData

| Feature        | Session           | TempData        | ViewData        |
| -------------- | ----------------- | --------------- | --------------- |
| Scope          | Multiple Requests | One Redirect    | Current Request |
| Storage        | Server            | Session/Cookies | Memory          |
| Lifetime       | Until Timeout     | One Request     | Current Request |
| Strongly Typed | No                | No              | No              |

---

# Common Use Cases

## Shopping Cart

```csharp
HttpContext.Session.SetString(
    "CartId",
    "CART123");
```

---

## Logged-In User Data

```csharp
HttpContext.Session.SetInt32(
    "UserId",
    user.Id);
```

---

## Multi-Step Form

```text
Step 1 → Personal Details
Step 2 → Address
Step 3 → Payment

Store progress in Session.
```

---

# Best Practices

### ✓ Store Small Data

Good:

```text
UserId
Role
Language
Theme
CartId
```

Avoid:

```text
Images
PDF Files
Large Objects
```

---

### ✓ Use Distributed Cache in Production

Avoid:

```csharp
AddDistributedMemoryCache()
```

for load-balanced applications.

Prefer:

```csharp
Redis
SQL Server
```

---

### ✓ Clear Session on Logout

```csharp
public async Task<IActionResult> Logout()
{
    HttpContext.Session.Clear();

    await _signInManager.SignOutAsync();

    return RedirectToAction("Login");
}
```

---

### ✓ Store IDs, Not Entire Objects

Prefer:

```csharp
Session["UserId"] = 101;
```

instead of storing large serialized objects.

---

# Interview Questions

### What is Session in ASP.NET Core MVC?

A server-side mechanism used to store user-specific data across multiple requests.

---

### Where is Session Data Stored?

On the server (memory, SQL Server, Redis, distributed cache).

---

### What is stored in the browser?

Only the Session ID cookie.

---

### Why is Session more secure than Cookies?

Because actual data remains on the server and is not exposed to the client.

---

### Which service is required before AddSession()?

```csharp
builder.Services.AddDistributedMemoryCache();
```

or another distributed cache provider.

---

### What happens when IdleTimeout is reached?

The session expires and all session data is removed.

---

# Real-World Usage in Your Projects

### La Caffeine Restaurant Web App

* Store Cart ID
* Store Table Reservation Progress
* Store Temporary Checkout Data

```csharp
HttpContext.Session.SetString("CartId", cartId);
```

### Smart Healthcare System

* Multi-step Doctor Registration
* Temporary Appointment Booking Data
* Store Selected Hospital ID

```csharp
HttpContext.Session.SetInt32(
    "HospitalId",
    hospitalId);
```

In modern ASP.NET Core applications, **authentication should be handled using ASP.NET Core Identity/JWT**, while **Session is used for temporary user-specific state**, such as carts, booking flows, and wizard-style forms.
