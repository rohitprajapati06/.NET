# Remote Validation in ASP.NET Core MVC

Remote Validation is a client-side validation technique that allows ASP.NET Core MVC to validate data by making an AJAX request to the server.

It is commonly used when validation requires checking data that exists in the database.

Examples:

* Username already exists
* Email already registered
* Hospital code already exists
* Doctor license number already registered

Instead of waiting for form submission, validation occurs while the user is filling the form.

---

# Why Use Remote Validation?

Built-in validations can only check local data:

```csharp
[Required]
[EmailAddress]
[StringLength(100)]
```

They cannot check:

```text
Is email already registered?
Is username unique?
Does doctor license already exist?
```

These validations require a database lookup.

Remote Validation solves this problem.

---

# How Remote Validation Works

```text
User enters email
       │
       ▼
Field loses focus
       │
       ▼
AJAX Request Sent
       │
       ▼
Controller Action Executes
       │
       ▼
Returns True/False
       │
       ▼
Validation Message Displayed
```

---

# Step 1: Add Remote Attribute

Model:

```csharp
using Microsoft.AspNetCore.Mvc;

public class RegisterViewModel
{
    [Required]
    [EmailAddress]

    [Remote(
        action: "VerifyEmail",
        controller: "Account")]
    public string Email { get; set; }
}
```

When Email changes, MVC automatically calls:

```text
/Account/VerifyEmail
```

---

# Step 2: Create Validation Action

Controller:

```csharp
using Microsoft.AspNetCore.Mvc;

public class AccountController : Controller
{
    [AcceptVerbs("GET", "POST")]
    public IActionResult VerifyEmail(string email)
    {
        bool emailExists =
            email == "admin@gmail.com";

        if (emailExists)
        {
            return Json(
                $"Email {email} is already registered.");
        }

        return Json(true);
    }
}
```

---

# Response Rules

### Valid

```csharp
return Json(true);
```

Means:

```text
Validation Passed
```

---

### Invalid

```csharp
return Json("Email already exists");
```

Means:

```text
Validation Failed
```

The string becomes the validation message.

---

# Example: Username Validation

## Model

```csharp
public class RegisterViewModel
{
    [Required]

    [Remote(
        "VerifyUsername",
        "Account")]
    public string Username { get; set; }
}
```

---

## Controller

```csharp
[AcceptVerbs("GET", "POST")]
public IActionResult VerifyUsername(
    string username)
{
    var usernames = new[]
    {
        "admin",
        "rohit",
        "john"
    };

    if (usernames.Contains(
        username.ToLower()))
    {
        return Json(
            "Username already taken");
    }

    return Json(true);
}
```

---

# Database Example

Real-world scenario:

```csharp
private readonly ApplicationDbContext _context;

public AccountController(
    ApplicationDbContext context)
{
    _context = context;
}

[AcceptVerbs("GET", "POST")]
public async Task<IActionResult> VerifyEmail(
    string email)
{
    bool exists =
        await _context.Users
            .AnyAsync(x => x.Email == email);

    if (exists)
    {
        return Json(
            "Email already registered");
    }

    return Json(true);
}
```

---

# Using AdditionalFields

Sometimes validation depends on multiple fields.

Example:

* FirstName + LastName combination must be unique.

---

## Model

```csharp
public class EmployeeViewModel
{
    public string FirstName { get; set; }

    [Remote(
        "VerifyEmployee",
        "Employee",
        AdditionalFields = nameof(FirstName))]
    public string LastName { get; set; }
}
```

---

## Controller

```csharp
public IActionResult VerifyEmployee(
    string firstName,
    string lastName)
{
    bool exists =
        firstName == "Rohit"
        && lastName == "Sharma";

    if (exists)
    {
        return Json(
            "Employee already exists");
    }

    return Json(true);
}
```

---

# Custom Error Message

```csharp
[Remote(
    "VerifyEmail",
    "Account",
    ErrorMessage =
        "Email already exists")]
public string Email { get; set; }
```

If the controller returns:

```csharp
Json(false)
```

MVC displays:

```text
Email already exists
```

---

# AcceptVerbs Attribute

Remote validation typically supports both GET and POST.

```csharp
[AcceptVerbs("GET", "POST")]
public IActionResult VerifyEmail(
    string email)
{
}
```

This allows validation regardless of HTTP method.

---

# Client-Side Requirements

Remote validation requires:

### jQuery

```html
<script src="~/lib/jquery/jquery.min.js"></script>
```

### jQuery Validation

```html
<script src="~/lib/jquery-validation/jquery.validate.min.js"></script>
```

### Unobtrusive Validation

```html
<script src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js"></script>
```

Typically included in:

```html
_ValidationScriptsPartial.cshtml
```

---

# Razor View Example

```html
<form asp-action="Register">

    <div>
        <label asp-for="Email"></label>

        <input asp-for="Email" />

        <span asp-validation-for="Email"></span>
    </div>

    <button type="submit">
        Register
    </button>

</form>

<partial name="_ValidationScriptsPartial" />
```

When the user leaves the Email field:

```text
AJAX request sent automatically
```

---

# Validation Flow

```text
User Types Email
       │
       ▼
Field Loses Focus
       │
       ▼
Remote Attribute Triggered
       │
       ▼
AJAX Request
       │
       ▼
Controller Validation
       │
       ▼
Json(true) / Json(error)
       │
       ▼
Validation Message Shown
```

---

# Advantages

### Better User Experience

User gets feedback immediately.

### Reduced Form Submissions

Invalid data is caught early.

### Database Validation

Can validate against live database records.

### Reusable

Same validation action can be used across forms.

---

# Limitations

### Requires JavaScript

Without JavaScript:

```text
Remote Validation does not run.
```

### Extra Server Requests

Every validation triggers an HTTP request.

### Not a Security Feature

Always validate again on the server when the form is submitted.

Never rely solely on Remote Validation.

---

# Remote Validation vs Custom Validation Attribute

| Feature              | Remote Validation    | Custom Attribute |
| -------------------- | -------------------- | ---------------- |
| Database Access      | ✅                    | ❌ Limited        |
| AJAX Based           | ✅                    | ❌                |
| Client Side Feedback | ✅                    | ❌                |
| Server Validation    | ⚠️ Should Revalidate | ✅                |
| Reusable             | ✅                    | ✅                |

---

# Remote Validation vs FluentValidation

| Feature              | Remote Validation | FluentValidation |
| -------------------- | ----------------- | ---------------- |
| Runs During Typing   | ✅                 | ❌                |
| Database Lookup      | ✅                 | ✅                |
| Client-Side Feedback | ✅                 | ❌                |
| Business Rules       | ❌                 | ✅                |
| CQRS Compatible      | ❌                 | ✅                |

---

# In Modern ASP.NET Core Projects

For MVC applications:

```text
Data Annotations
        +
Remote Validation
        +
Server Validation
```

For Clean Architecture + CQRS applications like your Smart Healthcare project:

```text
Client Validation
        +
Remote Validation (optional)
        +
FluentValidation
        +
Validation Pipeline Behavior
```

Use Remote Validation mainly for UI conveniences such as checking whether an email, username, doctor registration number, or hospital code already exists before the user submits the form. Always perform the same validation again on the server side before saving data.
