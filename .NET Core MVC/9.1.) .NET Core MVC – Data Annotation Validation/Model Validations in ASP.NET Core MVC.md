# Model Validation in ASP.NET Core MVC

Model Validation is the process of ensuring that the data sent by a user matches the rules defined in your model before it is processed, saved to the database, or used by the application.

ASP.NET Core automatically performs validation after Model Binding and before the controller action executes.

---

# Why Model Validation is Important

Without validation, users can submit:

* Empty required fields
* Invalid email addresses
* Negative prices
* Invalid dates
* Incorrect phone numbers
* Malicious or unexpected data

Validation helps maintain:

* Data Integrity
* Application Security
* Better User Experience
* Consistent Business Rules

---

# Request Processing Pipeline

When a request arrives:

```text
Client Request
       │
       ▼
Model Binding
       │
       ▼
Model Validation
       │
       ▼
ModelState
       │
       ▼
Controller Action
```

### Example Request

```json
{
  "email": "",
  "password": "123"
}
```

ASP.NET Core:

1. Creates the DTO object.
2. Executes validation attributes.
3. Stores validation errors in ModelState.
4. Executes controller action.

---

# Creating a Validated Model

```csharp
using System.ComponentModel.DataAnnotations;

public class RegisterDto
{
    [Required]
    public string FirstName { get; set; }

    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [MinLength(8)]
    public string Password { get; set; }
}
```

---

# Checking Validation in Controller

```csharp
[HttpPost]
public IActionResult Register(RegisterDto model)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    return Ok("User Registered");
}
```

---

# Example Validation Failure

### Request

```json
{
  "firstName": "",
  "email": "abc",
  "password": "123"
}
```

### Validation Errors

```json
{
  "FirstName": [
    "The FirstName field is required."
  ],
  "Email": [
    "The Email field is not a valid e-mail address."
  ],
  "Password": [
    "The field Password must be a string with a minimum length of 8."
  ]
}
```

---

# Understanding ModelState

ModelState stores:

* Submitted values
* Validation results
* Validation errors

### Example

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### Access Specific Error

```csharp
var errors = ModelState["Email"].Errors;
```

### Loop Through All Errors

```csharp
foreach (var state in ModelState)
{
    foreach (var error in state.Value.Errors)
    {
        Console.WriteLine(error.ErrorMessage);
    }
}
```

---

# Automatic Validation with `[ApiController]`

When using APIs:

```csharp
[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
}
```

ASP.NET Core automatically validates models.

### DTO

```csharp
public class LoginDto
{
    [Required]
    public string Email { get; set; }
}
```

### Request

```json
{
}
```

### Response

```json
{
  "errors": {
    "Email": [
      "The Email field is required."
    ]
  }
}
```

The controller action never executes because validation fails first.

---

# Custom Validation Message

```csharp
public class RegisterDto
{
    [Required(ErrorMessage = "Email is mandatory")]
    [EmailAddress(ErrorMessage = "Please enter valid email")]
    public string Email { get; set; }
}
```

Output:

```json
{
  "Email": [
    "Email is mandatory"
  ]
}
```

---

# Multiple Validations on a Property

```csharp
public class RegisterDto
{
    [Required]
    [StringLength(100)]
    [EmailAddress]
    public string Email { get; set; }
}
```

Validation executes all applicable rules.

---

# Conditional Business Validation

Sometimes Data Annotations are not enough.

Example:

* Doctor appointment date must be future date.
* Hospital must be approved.
* Booking slot should not exceed limit.

These are business rules and should be validated separately.

```csharp
if (appointmentDate <= DateTime.UtcNow)
{
    return BadRequest("Appointment date must be in future");
}
```

---

# Custom Validation Attribute

You can create your own validation attribute.

### Example

```csharp
using System.ComponentModel.DataAnnotations;

public class FutureDateAttribute : ValidationAttribute
{
    public override bool IsValid(object value)
    {
        if (value is DateTime date)
        {
            return date > DateTime.Now;
        }

        return false;
    }
}
```

### Usage

```csharp
public class AppointmentDto
{
    [FutureDate(ErrorMessage = "Appointment must be in future")]
    public DateTime AppointmentDate { get; set; }
}
```

---

# Implementing `IValidatableObject`

Used when validation depends on multiple properties.

```csharp
using System.ComponentModel.DataAnnotations;

public class RegisterDto : IValidatableObject
{
    public string Password { get; set; }

    public string ConfirmPassword { get; set; }

    public IEnumerable<ValidationResult> Validate(
        ValidationContext validationContext)
    {
        if (Password != ConfirmPassword)
        {
            yield return new ValidationResult(
                "Passwords do not match",
                new[] { nameof(ConfirmPassword) });
        }
    }
}
```

---

# Validation in Razor Views

### Model

```csharp
public class UserViewModel
{
    [Required]
    public string Name { get; set; }
}
```

### View

```html
<form asp-action="Create">

    <input asp-for="Name" />

    <span asp-validation-for="Name"></span>

    <button type="submit">
        Save
    </button>

</form>
```

Validation messages automatically appear.

---

# Server-Side vs Client-Side Validation

| Server-Side Validation | Client-Side Validation |
| ---------------------- | ---------------------- |
| Runs on server         | Runs in browser        |
| More secure            | Faster user feedback   |
| Cannot be bypassed     | Can be bypassed        |
| Required for all apps  | Optional enhancement   |

### Best Practice

Always use:

```text
Client-Side Validation
+
Server-Side Validation
```

Never rely only on client-side validation.

---

# Validation Flow in ASP.NET Core Web API

```text
HTTP Request
      │
      ▼
Model Binding
      │
      ▼
Data Annotation Validation
      │
      ▼
ModelState Created
      │
      ▼
ModelState.IsValid ?
      │
 ┌────┴────┐
 │         │
Yes       No
 │         │
 ▼         ▼
Controller 400 Bad Request
Action
```

---

# Model Validation in Clean Architecture + CQRS

For your Smart Healthcare project:

### Basic Validation

Use Data Annotations:

```csharp
public class RegisterDoctorCommand
{
    [Required]
    public string FirstName { get; set; }

    [Required]
    public string Email { get; set; }
}
```

### Advanced Validation

Use FluentValidation:

```csharp
public class RegisterDoctorCommandValidator
    : AbstractValidator<RegisterDoctorCommand>
{
    public RegisterDoctorCommandValidator()
    {
        RuleFor(x => x.FirstName)
            .NotEmpty();

        RuleFor(x => x.Email)
            .NotEmpty()
            .EmailAddress();
    }
}
```

### Recommended in Your Project

Since you're using:

* ASP.NET Core Web API
* Clean Architecture
* CQRS
* MediatR

A common enterprise approach is:

```text
Data Annotations
        +
FluentValidation
        +
Validation Pipeline Behavior
```

This allows validation to occur before handlers execute, keeping command handlers focused on business logic rather than input validation.
