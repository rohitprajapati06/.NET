# Data Annotations in ASP.NET Core MVC

Data Annotations are attributes used to **validate model properties** and **control how data is displayed** in ASP.NET Core MVC.

They help enforce business rules before data is processed or saved to the database.

---

# Why Use Data Annotations?

* Validate user input automatically
* Reduce manual validation code
* Improve data consistency
* Customize display names and formats
* Work with Model Binding and ModelState

---

# Common Data Annotation Attributes

## 1. Required

Makes a field mandatory.

```csharp
using System.ComponentModel.DataAnnotations;

public class User
{
    [Required]
    public string Name { get; set; }
}
```

### Valid

```json
{
  "name": "Rohit"
}
```

### Invalid

```json
{
  "name": ""
}
```

Error:

```text
The Name field is required.
```

---

## 2. StringLength

Sets minimum and maximum character limits.

```csharp
public class User
{
    [StringLength(50, MinimumLength = 3)]
    public string Name { get; set; }
}
```

### Valid

```text
Rohit
```

### Invalid

```text
Ro
```

Error:

```text
The field Name must be a string with a minimum length of 3 and a maximum length of 50.
```

---

## 3. MaxLength

Specifies maximum allowed length.

```csharp
public class Product
{
    [MaxLength(100)]
    public string ProductName { get; set; }
}
```

Useful with Entity Framework because it also affects database column size.

---

## 4. MinLength

Specifies minimum length.

```csharp
public class User
{
    [MinLength(6)]
    public string Password { get; set; }
}
```

---

## 5. Range

Validates numeric values within a range.

```csharp
public class Product
{
    [Range(1, 10000)]
    public decimal Price { get; set; }
}
```

### Valid

```text
500
```

### Invalid

```text
0
```

Error:

```text
The field Price must be between 1 and 10000.
```

---

## 6. EmailAddress

Validates email format.

```csharp
public class RegisterDto
{
    [EmailAddress]
    public string Email { get; set; }
}
```

### Valid

```text
rohit@gmail.com
```

### Invalid

```text
rohitgmail.com
```

---

## 7. Phone

Validates phone number format.

```csharp
public class User
{
    [Phone]
    public string PhoneNumber { get; set; }
}
```

---

## 8. Url

Validates URL format.

```csharp
public class Company
{
    [Url]
    public string Website { get; set; }
}
```

---

## 9. Compare

Used for matching two properties.

Commonly used for password confirmation.

```csharp
public class RegisterDto
{
    public string Password { get; set; }

    [Compare("Password")]
    public string ConfirmPassword { get; set; }
}
```

### Valid

```text
Password = Test@123
ConfirmPassword = Test@123
```

### Invalid

```text
Password = Test@123
ConfirmPassword = Test@321
```

---

## 10. RegularExpression

Validates using regex.

```csharp
public class User
{
    [RegularExpression(@"^[A-Za-z ]+$")]
    public string FullName { get; set; }
}
```

Allows only alphabets and spaces.

---

## 11. CreditCard

Validates credit card format.

```csharp
public class Payment
{
    [CreditCard]
    public string CardNumber { get; set; }
}
```

---

## 12. DataType

Specifies how data should be displayed.

```csharp
public class User
{
    [DataType(DataType.Password)]
    public string Password { get; set; }
}
```

Other examples:

```csharp
[DataType(DataType.Date)]

[DataType(DataType.EmailAddress)]

[DataType(DataType.Currency)]
```

---

## 13. Display

Changes display name.

```csharp
public class User
{
    [Display(Name = "Full Name")]
    public string Name { get; set; }
}
```

Instead of:

```text
Name
```

Displays:

```text
Full Name
```

---

## 14. DisplayFormat

Formats displayed data.

```csharp
public class Employee
{
    [DisplayFormat(DataFormatString = "{0:dd/MM/yyyy}")]
    public DateTime JoiningDate { get; set; }
}
```

Output:

```text
10/06/2026
```

---

## 15. Key

Marks primary key.

```csharp
public class Product
{
    [Key]
    public int ProductId { get; set; }
}
```

Used by Entity Framework.

---

## 16. ForeignKey

Defines relationship between entities.

```csharp
public class Order
{
    public int UserId { get; set; }

    [ForeignKey("UserId")]
    public User User { get; set; }
}
```

---

## 17. NotMapped

Prevents a property from being mapped to a database column.

```csharp
using System.ComponentModel.DataAnnotations.Schema;

public class User
{
    public string FirstName { get; set; }

    public string LastName { get; set; }

    [NotMapped]
    public string FullName
    {
        get
        {
            return FirstName + " " + LastName;
        }
    }
}
```

---

# Custom Error Messages

You can customize validation messages.

```csharp
public class RegisterDto
{
    [Required(ErrorMessage = "Email is required")]
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }
}
```

---

# Data Annotations in DTO Example

```csharp
public class RegisterDto
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

# Validation in Controller

```csharp
[HttpPost]
public IActionResult Register(RegisterDto model)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    return Ok("Registration Successful");
}
```

---

# How Validation Works Internally

### Step 1

Request comes:

```json
{
  "email": "",
  "password": "123"
}
```

### Step 2

Model Binding creates object.

```csharp
RegisterDto dto
```

### Step 3

Validation attributes execute.

```csharp
[Required]
[MinLength(8)]
```

### Step 4

Errors added to ModelState.

```csharp
ModelState.IsValid == false
```

### Step 5

Controller returns validation errors.

```csharp
return BadRequest(ModelState);
```

---

# Data Annotations vs Fluent Validation

| Data Annotations        | FluentValidation             |
| ----------------------- | ---------------------------- |
| Built into .NET         | External package             |
| Simple validations      | Complex validations          |
| Attribute based         | Fluent syntax                |
| Good for small projects | Better for large projects    |
| Validation inside model | Validation in separate class |

Example FluentValidation:

```csharp
RuleFor(x => x.Email)
    .NotEmpty()
    .EmailAddress();

RuleFor(x => x.Password)
    .MinimumLength(8);
```

---

# Best Practices

### Use Data Annotations For

* DTO Validation
* Simple Forms
* Login/Register APIs
* Small to Medium Projects

### Use FluentValidation For

* Enterprise Applications
* CQRS + MediatR Projects
* Complex Business Rules
* Clean Architecture

