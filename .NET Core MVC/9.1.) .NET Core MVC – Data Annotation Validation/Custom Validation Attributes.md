# Custom Validation Attributes in ASP.NET Core MVC

Custom Validation Attributes allow you to create your own validation rules when built-in Data Annotation attributes (`[Required]`, `[EmailAddress]`, `[Range]`, etc.) are not sufficient.

They are useful for implementing business-specific validation logic.

---

# Why Use Custom Validation Attributes?

Built-in attributes can validate:

* Required fields
* Email format
* Phone numbers
* Length limits
* Numeric ranges

But they cannot easily validate rules such as:

* Appointment date must be in the future
* Age must be at least 18
* Hospital code must follow a custom format
* Doctor license number must match organization standards
* Booking date cannot be on weekends

For such scenarios, create custom validation attributes.

---

# Creating a Custom Validation Attribute

All custom validation attributes inherit from:

```csharp
ValidationAttribute
```

Namespace:

```csharp
using System.ComponentModel.DataAnnotations;
```

---

# Example 1: Future Date Validation

Suppose appointments can only be booked for future dates.

## Step 1: Create Custom Attribute

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class FutureDateAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        if (value == null)
        {
            return ValidationResult.Success;
        }

        if (value is DateTime date)
        {
            if (date > DateTime.Now)
            {
                return ValidationResult.Success;
            }
        }

        return new ValidationResult(
            ErrorMessage ?? "Date must be in the future.");
    }
}
```

---

## Step 2: Apply Attribute

```csharp
public class AppointmentDto
{
    [FutureDate]
    public DateTime AppointmentDate { get; set; }
}
```

---

## Step 3: Custom Error Message

```csharp
public class AppointmentDto
{
    [FutureDate(
        ErrorMessage = "Appointment date must be in the future")]
    public DateTime AppointmentDate { get; set; }
}
```

---

# Example 2: Minimum Age Validation

User must be at least 18 years old.

## Custom Attribute

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class MinimumAgeAttribute : ValidationAttribute
{
    private readonly int _minimumAge;

    public MinimumAgeAttribute(int minimumAge)
    {
        _minimumAge = minimumAge;
    }

    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        if (value == null)
        {
            return ValidationResult.Success;
        }

        var birthDate = (DateTime)value;

        var age = DateTime.Today.Year - birthDate.Year;

        if (birthDate.Date > DateTime.Today.AddYears(-age))
        {
            age--;
        }

        if (age >= _minimumAge)
        {
            return ValidationResult.Success;
        }

        return new ValidationResult(
            $"Age must be at least {_minimumAge} years.");
    }
}
```

---

## Usage

```csharp
public class RegisterPatientDto
{
    [MinimumAge(18)]
    public DateTime DateOfBirth { get; set; }
}
```

---

# Example 3: Hospital Code Validation

Hospital codes must start with "HSP".

## Attribute

```csharp
using System.ComponentModel.DataAnnotations;

public class HospitalCodeAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        var code = value?.ToString();

        if (!string.IsNullOrWhiteSpace(code)
            && code.StartsWith("HSP"))
        {
            return ValidationResult.Success;
        }

        return new ValidationResult(
            "Hospital code must start with HSP.");
    }
}
```

---

## Usage

```csharp
public class HospitalDto
{
    [HospitalCode]
    public string HospitalCode { get; set; }
}
```

---

# Accessing Other Properties

Sometimes validation depends on another property.

Example:

* EndDate must be greater than StartDate

---

## Custom Attribute

```csharp
using System;
using System.ComponentModel.DataAnnotations;
using System.Reflection;

public class DateGreaterThanAttribute : ValidationAttribute
{
    private readonly string _comparisonProperty;

    public DateGreaterThanAttribute(
        string comparisonProperty)
    {
        _comparisonProperty = comparisonProperty;
    }

    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        PropertyInfo property =
            validationContext.ObjectType
                .GetProperty(_comparisonProperty);

        if (property == null)
        {
            return new ValidationResult(
                $"Unknown property: {_comparisonProperty}");
        }

        var comparisonValue =
            (DateTime)property.GetValue(
                validationContext.ObjectInstance);

        var currentValue = (DateTime)value;

        if (currentValue > comparisonValue)
        {
            return ValidationResult.Success;
        }

        return new ValidationResult(
            ErrorMessage ??
            $"{validationContext.DisplayName} must be greater than {_comparisonProperty}");
    }
}
```

---

## Usage

```csharp
public class LeaveRequestDto
{
    public DateTime StartDate { get; set; }

    [DateGreaterThan(
        "StartDate",
        ErrorMessage = "End Date must be after Start Date")]
    public DateTime EndDate { get; set; }
}
```

---

# Using ValidationContext

`ValidationContext` provides information about:

```csharp
validationContext.ObjectInstance
validationContext.ObjectType
validationContext.DisplayName
validationContext.MemberName
```

Example:

```csharp
protected override ValidationResult IsValid(
    object value,
    ValidationContext validationContext)
{
    var model = validationContext.ObjectInstance;

    return ValidationResult.Success;
}
```

Useful when validation depends on multiple properties.

---

# Dependency Injection Inside Validation Attribute

Direct Dependency Injection is not supported through constructors because attributes must use compile-time constants.

However, services can be resolved through `ValidationContext`.

---

## Example

```csharp
protected override ValidationResult IsValid(
    object value,
    ValidationContext validationContext)
{
    var service =
        validationContext.GetService(typeof(IMyService))
            as IMyService;

    // use service

    return ValidationResult.Success;
}
```

---

# Alternative: IValidatableObject

For complex model-level validation, `IValidatableObject` is often cleaner.

```csharp
using System.ComponentModel.DataAnnotations;

public class AppointmentDto : IValidatableObject
{
    public DateTime StartDate { get; set; }

    public DateTime EndDate { get; set; }

    public IEnumerable<ValidationResult> Validate(
        ValidationContext validationContext)
    {
        if (EndDate <= StartDate)
        {
            yield return new ValidationResult(
                "End date must be after start date",
                new[] { nameof(EndDate) });
        }
    }
}
```

---

# Validation Flow

```text
HTTP Request
      │
      ▼
Model Binding
      │
      ▼
Custom Validation Attribute Executes
      │
      ▼
ModelState Updated
      │
      ▼
Controller Action
```

---

# Example in Controller

```csharp
[HttpPost]
public IActionResult CreateAppointment(
    AppointmentDto model)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    return Ok();
}
```

---

# Folder Structure (Recommended)

For your Smart Healthcare project:

```text
Application
│
├── Common
│   ├── Validation
│   │   ├── FutureDateAttribute.cs
│   │   ├── MinimumAgeAttribute.cs
│   │   └── DateGreaterThanAttribute.cs
│
├── Features
│   ├── Appointments
│   ├── Doctors
│   └── Patients
```

---

# Custom Validation vs FluentValidation

| Feature                    | Custom Attribute | FluentValidation |
| -------------------------- | ---------------- | ---------------- |
| Simple Property Validation | ✅                | ✅                |
| Reusable Across Models     | ✅                | ✅                |
| Cross-Property Validation  | ⚠️ Possible      | ✅ Easy           |
| Dependency Injection       | ⚠️ Limited       | ✅ Excellent      |
| Complex Business Rules     | ❌ Not Ideal      | ✅ Best Choice    |
| Clean Architecture         | ⚠️ Limited       | ✅ Preferred      |

---

# Recommended for Your Smart Healthcare Project

Use:

### Custom Validation Attributes

For reusable field-level rules:

```csharp
[FutureDate]
[MinimumAge(18)]
[HospitalCode]
```

### FluentValidation

For business rules:

```csharp
Appointment slot availability
Doctor approval checks
Duplicate email checks
Hospital existence validation
Maximum bookings per slot
```

In a Clean Architecture + CQRS + MediatR project, FluentValidation should handle most business validation, while custom validation attributes are best reserved for reusable property-level validation rules.
