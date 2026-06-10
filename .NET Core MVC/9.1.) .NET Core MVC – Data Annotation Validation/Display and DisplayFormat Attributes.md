# Display and DisplayFormat Attributes in ASP.NET Core MVC

The **Display** and **DisplayFormat** attributes are Data Annotation attributes used to control how model properties are displayed in MVC views, forms, tables, and validation messages.

They do **not perform validation**. Instead, they improve the presentation of data.

Namespace:

```csharp
using System.ComponentModel.DataAnnotations;
```

---

# 1. Display Attribute

The `Display` attribute is used to:

* Change property labels
* Provide friendly names
* Specify descriptions
* Set display order
* Group fields

## Syntax

```csharp
[Display(Name = "Custom Name")]
public string PropertyName { get; set; }
```

---

## Example Without Display

```csharp
public class Employee
{
    public string FirstName { get; set; }
}
```

Razor:

```html
<label asp-for="FirstName"></label>
```

Output:

```text
FirstName
```

---

## Example With Display

```csharp
public class Employee
{
    [Display(Name = "First Name")]
    public string FirstName { get; set; }
}
```

Razor:

```html
<label asp-for="FirstName"></label>
```

Output:

```text
First Name
```

---

# Display in Validation Messages

```csharp
public class RegisterDto
{
    [Display(Name = "Email Address")]

    [Required]
    public string Email { get; set; }
}
```

Validation Error:

```text
The Email Address field is required.
```

Instead of:

```text
The Email field is required.
```

---

# Display Description

```csharp
public class Doctor
{
    [Display(
        Name = "License Number",
        Description = "Government-issued doctor license")]
    public string LicenseNumber { get; set; }
}
```

Useful for tooltips and documentation.

---

# Display Prompt

```csharp
public class User
{
    [Display(
        Name = "Email",
        Prompt = "Enter your email")]
    public string Email { get; set; }
}
```

Can be used as placeholder text.

---

# Display Order

Controls display sequence.

```csharp
public class Employee
{
    [Display(Order = 1)]
    public string FirstName { get; set; }

    [Display(Order = 2)]
    public string LastName { get; set; }

    [Display(Order = 3)]
    public string Email { get; set; }
}
```

---

# DisplayFormat Attribute

`DisplayFormat` controls how values are displayed.

Commonly used for:

* Dates
* Currency
* Numbers
* Percentages

---

## Syntax

```csharp
[DisplayFormat(DataFormatString = "...")]
public propertyType PropertyName { get; set; }
```

---

# Formatting Dates

## Example

```csharp
public class Employee
{
    [DisplayFormat(
        DataFormatString = "{0:dd/MM/yyyy}")]
    public DateTime JoiningDate { get; set; }
}
```

Value:

```csharp
2026-06-10
```

Output:

```text
10/06/2026
```

---

## Other Date Formats

### Month-Day-Year

```csharp
[DisplayFormat(
    DataFormatString = "{0:MM-dd-yyyy}")]
```

Output:

```text
06-10-2026
```

---

### Long Date

```csharp
[DisplayFormat(
    DataFormatString = "{0:D}")]
```

Output:

```text
Wednesday, 10 June 2026
```

---

### Short Date

```csharp
[DisplayFormat(
    DataFormatString = "{0:d}")]
```

Output:

```text
10/06/2026
```

---

# Formatting Currency

```csharp
public class Product
{
    [DisplayFormat(
        DataFormatString = "{0:C}")]
    public decimal Price { get; set; }
}
```

Output:

```text
₹5,000.00
```

(Culture dependent)

---

# Formatting Numbers

## Two Decimal Places

```csharp
[DisplayFormat(
    DataFormatString = "{0:N2}")]
public decimal Amount { get; set; }
```

Output:

```text
1,234.56
```

---

## No Decimal Places

```csharp
[DisplayFormat(
    DataFormatString = "{0:N0}")]
```

Output:

```text
1,235
```

---

# Formatting Percentages

```csharp
[DisplayFormat(
    DataFormatString = "{0:P}")]
public decimal SuccessRate { get; set; }
```

Value:

```csharp
0.85
```

Output:

```text
85.00 %
```

---

# ApplyFormatInEditMode

By default, formatting is applied only when displaying data.

To apply formatting in edit controls:

```csharp
[DisplayFormat(
    DataFormatString = "{0:yyyy-MM-dd}",
    ApplyFormatInEditMode = true)]
public DateTime DateOfBirth { get; set; }
```

Useful for HTML date inputs.

---

# Example: Employee Model

```csharp
public class Employee
{
    [Display(Name = "Employee Name")]
    public string Name { get; set; }

    [Display(Name = "Date of Birth")]

    [DisplayFormat(
        DataFormatString = "{0:dd/MM/yyyy}")]
    public DateTime DateOfBirth { get; set; }

    [Display(Name = "Salary")]

    [DisplayFormat(
        DataFormatString = "{0:C}")]
    public decimal Salary { get; set; }
}
```

---

# Razor View Example

```html
<div>
    <label asp-for="Name"></label>
    <input asp-for="Name" />
</div>

<div>
    <label asp-for="DateOfBirth"></label>
    <input asp-for="DateOfBirth" />
</div>

<div>
    <label asp-for="Salary"></label>
    <input asp-for="Salary" />
</div>
```

Output Labels:

```text
Employee Name
Date of Birth
Salary
```

---

# Display vs DisplayName

### Display

```csharp
[Display(Name = "Full Name")]
```

Supports:

* Name
* Description
* Prompt
* Order
* GroupName

---

### DisplayName

```csharp
[DisplayName("Full Name")]
```

Namespace:

```csharp
using System.ComponentModel;
```

Supports only:

```text
Display Name
```

No advanced options.

---

# Display vs DisplayFormat

| Attribute     | Purpose                   |
| ------------- | ------------------------- |
| Display       | Controls label text       |
| DisplayFormat | Controls value formatting |

Example:

```csharp
[Display(Name = "Joining Date")]

[DisplayFormat(
    DataFormatString = "{0:dd/MM/yyyy}")]
public DateTime JoiningDate { get; set; }
```

Output:

```text
Joining Date : 10/06/2026
```

---

# Common DataFormatString Values

| Format           | Example Output          |
| ---------------- | ----------------------- |
| `{0:d}`          | 10/06/2026              |
| `{0:D}`          | Wednesday, 10 June 2026 |
| `{0:t}`          | 14:30                   |
| `{0:T}`          | 14:30:45                |
| `{0:C}`          | ₹5,000.00               |
| `{0:N}`          | 1,234.56                |
| `{0:N0}`         | 1,235                   |
| `{0:P}`          | 85.00 %                 |
| `{0:yyyy-MM-dd}` | 2026-06-10              |

---

# Best Practices

### Use Display For

* User-friendly labels
* Better validation messages
* Professional UI

```csharp
[Display(Name = "Doctor Name")]
```

---

### Use DisplayFormat For

* Dates
* Currency
* Percentages
* Financial values

```csharp
[DisplayFormat(DataFormatString = "{0:dd/MM/yyyy}")]
```

---

# In Real ASP.NET Core Projects

For entities and DTOs in applications such as your Smart Healthcare system:

```csharp
public class DoctorDto
{
    [Display(Name = "Doctor Name")]
    public string FullName { get; set; }

    [Display(Name = "Consultation Fee")]
    [DisplayFormat(DataFormatString = "{0:C}")]
    public decimal ConsultationFee { get; set; }

    [Display(Name = "Registration Date")]
    [DisplayFormat(DataFormatString = "{0:dd/MM/yyyy}")]
    public DateTime CreatedAt { get; set; }
}
```

This produces cleaner forms, tables, reports, and validation messages without changing the underlying data or validation logic.
