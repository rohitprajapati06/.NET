# Data Annotation Attributes in ASP.NET Core MVC

Data Annotation Attributes are built-in attributes in ASP.NET Core used to:

* Validate user input
* Configure model behavior
* Customize display information
* Define database mappings (with Entity Framework Core)

They are applied directly to model properties.

---

# Namespace

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
```

---

# Validation Attributes

## 1. Required

Makes a property mandatory.

```csharp
public class User
{
    [Required]
    public string Name { get; set; }
}
```

### Custom Message

```csharp
[Required(ErrorMessage = "Name is required")]
public string Name { get; set; }
```

---

## 2. StringLength

Sets minimum and maximum character limits.

```csharp
[StringLength(50)]
public string Name { get; set; }
```

### Minimum and Maximum

```csharp
[StringLength(50, MinimumLength = 3)]
public string Name { get; set; }
```

Valid:

```text
Rohit
```

Invalid:

```text
Ro
```

---

## 3. MaxLength

Defines maximum length.

```csharp
[MaxLength(100)]
public string Description { get; set; }
```

Also affects database column size in EF Core.

---

## 4. MinLength

Defines minimum length.

```csharp
[MinLength(8)]
public string Password { get; set; }
```

---

## 5. Range

Validates numeric values.

```csharp
[Range(1, 100)]
public int Age { get; set; }
```

Valid:

```text
25
```

Invalid:

```text
150
```

---

## 6. EmailAddress

Validates email format.

```csharp
[EmailAddress]
public string Email { get; set; }
```

Valid:

```text
rohit@gmail.com
```

Invalid:

```text
rohitgmail.com
```

---

## 7. Phone

Validates phone number.

```csharp
[Phone]
public string PhoneNumber { get; set; }
```

---

## 8. Url

Validates URLs.

```csharp
[Url]
public string Website { get; set; }
```

Example:

```text
https://example.com
```

---

## 9. CreditCard

Validates credit card numbers.

```csharp
[CreditCard]
public string CardNumber { get; set; }
```

---

## 10. Compare

Compares two properties.

Commonly used for password confirmation.

```csharp
public class RegisterDto
{
    public string Password { get; set; }

    [Compare("Password")]
    public string ConfirmPassword { get; set; }
}
```

---

## 11. RegularExpression

Validates input using regex.

```csharp
[RegularExpression(@"^[A-Za-z ]+$")]
public string FullName { get; set; }
```

Allows only letters and spaces.

---

# Display Attributes

## 12. Display

Changes display name.

```csharp
[Display(Name = "Full Name")]
public string Name { get; set; }
```

Output:

```text
Full Name
```

instead of:

```text
Name
```

---

## 13. DisplayFormat

Formats displayed values.

```csharp
[DisplayFormat(DataFormatString = "{0:dd/MM/yyyy}")]
public DateTime DateOfBirth { get; set; }
```

Output:

```text
10/06/2026
```

---

## 14. DataType

Specifies data type for rendering.

```csharp
[DataType(DataType.Password)]
public string Password { get; set; }
```

Other options:

```csharp
[DataType(DataType.Date)]

[DataType(DataType.Time)]

[DataType(DataType.EmailAddress)]

[DataType(DataType.Currency)]

[DataType(DataType.MultilineText)]
```

---

## 15. ScaffoldColumn

Controls whether a property appears in generated UI.

```csharp
[ScaffoldColumn(false)]
public string InternalCode { get; set; }
```

---

# Database Mapping Attributes (EF Core)

## 16. Key

Defines primary key.

```csharp
[Key]
public int Id { get; set; }
```

---

## 17. ForeignKey

Defines relationship.

```csharp
public int HospitalId { get; set; }

[ForeignKey("HospitalId")]
public Hospital Hospital { get; set; }
```

---

## 18. NotMapped

Excludes property from database mapping.

```csharp
[NotMapped]
public string FullName
{
    get
    {
        return FirstName + " " + LastName;
    }
}
```

No column will be created.

---

## 19. Column

Customizes database column.

```csharp
[Column("Doctor_Name")]
public string Name { get; set; }
```

---

## 20. Table

Maps entity to a specific table.

```csharp
[Table("Doctors")]
public class Doctor
{
}
```

---

## 21. DatabaseGenerated

Controls value generation.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public int Id { get; set; }
```

Options:

```csharp
Identity
Computed
None
```

---

# Example: Registration DTO

```csharp
public class RegisterUserDto
{
    [Required]
    [StringLength(100)]
    public string FirstName { get; set; }

    [Required]
    [StringLength(100)]
    public string LastName { get; set; }

    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [MinLength(8)]
    public string Password { get; set; }

    [Compare("Password")]
    public string ConfirmPassword { get; set; }
}
```

---

# Example: Doctor Entity

```csharp
public class Doctor
{
    [Key]
    public Guid Id { get; set; }

    [Required]
    [MaxLength(150)]
    public string FullName { get; set; }

    [EmailAddress]
    public string Email { get; set; }

    [Phone]
    public string PhoneNumber { get; set; }

    [NotMapped]
    public string DisplayName
    {
        get
        {
            return $"Dr. {FullName}";
        }
    }
}
```

---

# Validation Execution Flow

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
ModelState
      │
      ▼
Controller Action
```

Example:

```csharp
[HttpPost]
public IActionResult Register(RegisterUserDto dto)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    return Ok();
}
```

---

# Most Frequently Used Attributes in Real Projects

| Attribute         | Purpose               |
| ----------------- | --------------------- |
| Required          | Mandatory field       |
| StringLength      | Min/Max characters    |
| MaxLength         | Database column size  |
| MinLength         | Password validation   |
| EmailAddress      | Email validation      |
| Phone             | Phone validation      |
| Range             | Numeric validation    |
| Compare           | Confirm password      |
| RegularExpression | Custom validation     |
| Display           | UI display name       |
| Key               | Primary key           |
| ForeignKey        | Relationships         |
| NotMapped         | Ignore property in DB |

---

# In Enterprise ASP.NET Core Projects

For projects like your Smart Healthcare application using:

* ASP.NET Core Web API
* Clean Architecture
* CQRS
* MediatR
* Entity Framework Core

A typical approach is:

```text
Data Annotations
       ↓
Basic Input Validation

FluentValidation
       ↓
Business Rule Validation

Validation Pipeline Behavior
       ↓
Automatic Validation Before Handler Execution
```

Use Data Annotations for simple field validation (`Required`, `EmailAddress`, `MaxLength`) and use FluentValidation for complex business rules such as appointment scheduling, doctor approval checks, slot availability, and hospital-specific constraints.
