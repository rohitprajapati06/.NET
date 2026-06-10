# BindNever and BindRequired Attributes in ASP.NET Core MVC

`BindNever` and `BindRequired` are model binding attributes used to control how ASP.NET Core binds incoming request data to model properties.

Namespace:

```csharp
using Microsoft.AspNetCore.Mvc.ModelBinding;
```

These attributes are related to **Model Binding**, not validation.

---

# What is Model Binding?

Model Binding automatically maps request data to action parameters or model properties.

Example Request:

```json
{
  "name": "Rohit",
  "email": "rohit@gmail.com"
}
```

Model:

```csharp
public class UserDto
{
    public string Name { get; set; }

    public string Email { get; set; }
}
```

ASP.NET Core automatically creates:

```csharp
var user = new UserDto
{
    Name = "Rohit",
    Email = "rohit@gmail.com"
};
```

This process is called **Model Binding**.

---

# BindNever Attribute

`[BindNever]` tells ASP.NET Core:

> "Never bind this property from incoming requests."

Even if the client sends a value, ASP.NET Core ignores it.

---

# Why Use BindNever?

To protect sensitive fields such as:

* Id
* CreatedDate
* CreatedBy
* IsAdmin
* Role
* ApprovalStatus
* System-generated values

This helps prevent **over-posting attacks**.

---

# Example Without BindNever

```csharp
public class UserDto
{
    public string Name { get; set; }

    public bool IsAdmin { get; set; }
}
```

Request:

```json
{
  "name": "Rohit",
  "isAdmin": true
}
```

Result:

```csharp
user.IsAdmin = true;
```

A malicious user can elevate privileges.

---

# Example With BindNever

```csharp
using Microsoft.AspNetCore.Mvc.ModelBinding;

public class UserDto
{
    public string Name { get; set; }

    [BindNever]
    public bool IsAdmin { get; set; }
}
```

Request:

```json
{
  "name": "Rohit",
  "isAdmin": true
}
```

Result:

```csharp
user.Name = "Rohit";
user.IsAdmin = false;
```

The incoming value is ignored.

---

# Real Example: Order Creation

```csharp
public class CreateOrderDto
{
    public string ProductName { get; set; }

    public int Quantity { get; set; }

    [BindNever]
    public decimal TotalAmount { get; set; }
}
```

Request:

```json
{
  "productName": "Laptop",
  "quantity": 2,
  "totalAmount": 1
}
```

Result:

```csharp
dto.TotalAmount = 0;
```

You calculate it on the server:

```csharp
dto.TotalAmount = product.Price * dto.Quantity;
```

---

# BindNever with Entity Models

```csharp
public class Doctor
{
    public Guid Id { get; set; }

    public string Name { get; set; }

    [BindNever]
    public DateTime CreatedAt { get; set; }
}
```

Client cannot modify:

```text
CreatedAt
```

through HTTP requests.

---

# BindRequired Attribute

`[BindRequired]` tells ASP.NET Core:

> "This property must be supplied during model binding."

If the property is missing, model binding fails.

---

# Difference Between Required and BindRequired

Many developers confuse these attributes.

## Required

```csharp
[Required]
public string Email { get; set; }
```

Validation attribute.

Checks:

```text
After model binding completes
```

---

## BindRequired

```csharp
[BindRequired]
public string Email { get; set; }
```

Model binding attribute.

Checks:

```text
During model binding
```

---

# Example Using BindRequired

```csharp
public class RegisterDto
{
    [BindRequired]
    public string Email { get; set; }
}
```

Request:

```json
{
}
```

ModelState Error:

```text
A value for the 'Email' property was not provided.
```

---

# Example With Required

```csharp
public class RegisterDto
{
    [Required]
    public string Email { get; set; }
}
```

Request:

```json
{
}
```

ModelState Error:

```text
The Email field is required.
```

The timing is different, but both fail.

---

# BindRequired Example

```csharp
public class AppointmentDto
{
    [BindRequired]
    public DateTime AppointmentDate { get; set; }
}
```

Request:

```json
{
}
```

Result:

```csharp
ModelState.IsValid == false
```

Error:

```text
A value for the 'AppointmentDate' property was not provided.
```

---

# Controller Example

```csharp
[HttpPost]
public IActionResult Create(RegisterDto dto)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    return Ok();
}
```

---

# BindNever + BindRequired Together

Example:

```csharp
public class UserDto
{
    [BindRequired]
    public string Name { get; set; }

    [BindNever]
    public bool IsAdmin { get; set; }
}
```

Request:

```json
{
  "name": "Rohit",
  "isAdmin": true
}
```

Result:

```csharp
Name = "Rohit"
IsAdmin = false
```

ModelState:

```text
Valid
```

---

# Over-Posting Attack Prevention

Suppose your entity:

```csharp
public class User
{
    public string Name { get; set; }

    public string Email { get; set; }

    public string Role { get; set; }
}
```

Malicious request:

```json
{
  "name": "Rohit",
  "email": "rohit@gmail.com",
  "role": "Admin"
}
```

Without protection:

```csharp
Role = Admin
```

With:

```csharp
[BindNever]
public string Role { get; set; }
```

Result:

```csharp
Role = null
```

---

# Validation Flow

```text
HTTP Request
      │
      ▼
Model Binding
      │
      ├── BindRequired Check
      │
      ├── BindNever Ignore
      │
      ▼
Model Created
      │
      ▼
Validation Attributes
      │
      ▼
ModelState
      │
      ▼
Controller Action
```

---

# Bind Attribute vs BindNever

### Bind

```csharp
[Bind("Name,Email")]
```

Only specific properties are allowed.

---

### BindNever

```csharp
[BindNever]
```

Specific property is forbidden.

---

# Best Practices

## Use BindNever For

```csharp
Id
CreatedAt
UpdatedAt
Role
ApprovalStatus
IsAdmin
CreatedBy
```

Example:

```csharp
[BindNever]
public DateTime CreatedAt { get; set; }
```

---

## Use BindRequired For

Properties that must always be supplied:

```csharp
[BindRequired]
public string Email { get; set; }
```

---

# Important Note for Modern Web APIs

In modern ASP.NET Core Web APIs, especially with:

* Clean Architecture
* CQRS
* MediatR
* DTOs

`BindNever` and `BindRequired` are used less frequently because DTOs are designed to expose only the properties clients should send.

Example:

```csharp
public class CreateDoctorCommand
{
    public string FullName { get; set; }

    public string Email { get; set; }
}
```

Instead of:

```csharp
public class Doctor
{
    public string FullName { get; set; }

    [BindNever]
    public DateTime CreatedAt { get; set; }
}
```

The preferred approach is:

1. Use DTOs/Commands that contain only client-editable fields.
2. Set system fields (`CreatedAt`, `CreatedBy`, `ApprovalStatus`) on the server.
3. Use `[Required]` or FluentValidation for validation.
4. Reserve `BindNever` and `BindRequired` mainly for MVC forms and specialized model-binding scenarios.

For your Smart Healthcare project (Clean Architecture + CQRS + MediatR), DTO/Command design and FluentValidation are usually more important than `BindNever` and `BindRequired`, but understanding them helps when working with MVC forms and model binding internals.
