# Blacklist and Whitelist Checks using Data Annotations in ASP.NET Core MVC

ASP.NET Core does not provide built-in `[Blacklist]` or `[Whitelist]` Data Annotation attributes. However, you can create **custom validation attributes** to enforce blacklist or whitelist rules.

These are commonly used for:

* Allowed email domains
* Blocked email domains
* Allowed usernames
* Blocked usernames
* Allowed file extensions
* Restricted words in comments
* Approved hospital codes

---

# What is a Whitelist?

A whitelist contains values that are allowed.

Example:

```text
gmail.com
outlook.com
yahoo.com
```

Only these values are accepted.

---

# What is a Blacklist?

A blacklist contains values that are prohibited.

Example:

```text
admin
root
superuser
```

These values are rejected.

---

# Whitelist Validation Example

Suppose users can register only with specific email domains.

## Create Custom Attribute

```csharp
using System.ComponentModel.DataAnnotations;

public class AllowedEmailDomainAttribute
    : ValidationAttribute
{
    private readonly string[] _allowedDomains;

    public AllowedEmailDomainAttribute(
        params string[] allowedDomains)
    {
        _allowedDomains = allowedDomains;
    }

    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        if (value == null)
        {
            return ValidationResult.Success;
        }

        string email = value.ToString();

        string domain = email.Split('@').Last();

        if (_allowedDomains.Contains(
            domain,
            StringComparer.OrdinalIgnoreCase))
        {
            return ValidationResult.Success;
        }

        return new ValidationResult(
            $"Only the following domains are allowed: {string.Join(", ", _allowedDomains)}");
    }
}
```

---

## Usage

```csharp
public class RegisterDto
{
    [AllowedEmailDomain(
        "gmail.com",
        "outlook.com",
        "yahoo.com")]
    public string Email { get; set; }
}
```

---

## Valid

```text
rohit@gmail.com
```

---

## Invalid

```text
rohit@unknown.com
```

Error:

```text
Only the following domains are allowed:
gmail.com, outlook.com, yahoo.com
```

---

# Blacklist Validation Example

Suppose some usernames are prohibited.

---

## Create Attribute

```csharp
using System.ComponentModel.DataAnnotations;

public class BlacklistAttribute
    : ValidationAttribute
{
    private readonly string[] _blockedValues;

    public BlacklistAttribute(
        params string[] blockedValues)
    {
        _blockedValues = blockedValues;
    }

    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        if (value == null)
        {
            return ValidationResult.Success;
        }

        string input = value.ToString();

        if (_blockedValues.Contains(
            input,
            StringComparer.OrdinalIgnoreCase))
        {
            return new ValidationResult(
                $"{input} is not allowed.");
        }

        return ValidationResult.Success;
    }
}
```

---

## Usage

```csharp
public class RegisterDto
{
    [Blacklist(
        "admin",
        "root",
        "superuser")]
    public string Username { get; set; }
}
```

---

## Valid

```text
Rohit123
```

---

## Invalid

```text
admin
```

Error:

```text
admin is not allowed.
```

---

# Whitelist for File Extensions

Example:

Only PDF and DOCX files are allowed.

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.AspNetCore.Http;

public class AllowedFileExtensionsAttribute
    : ValidationAttribute
{
    private readonly string[] _extensions;

    public AllowedFileExtensionsAttribute(
        params string[] extensions)
    {
        _extensions = extensions;
    }

    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        var file = value as IFormFile;

        if (file == null)
        {
            return ValidationResult.Success;
        }

        var extension =
            Path.GetExtension(file.FileName);

        if (_extensions.Contains(
            extension,
            StringComparer.OrdinalIgnoreCase))
        {
            return ValidationResult.Success;
        }

        return new ValidationResult(
            $"Allowed extensions: {string.Join(", ", _extensions)}");
    }
}
```

---

## Usage

```csharp
public class UploadDocumentDto
{
    [AllowedFileExtensions(
        ".pdf",
        ".docx")]
    public IFormFile File { get; set; }
}
```

---

# Blacklist for Restricted Words

Suppose patient feedback should not contain offensive words.

```csharp
public class RestrictedWordsAttribute
    : ValidationAttribute
{
    private readonly string[] _words;

    public RestrictedWordsAttribute(
        params string[] words)
    {
        _words = words;
    }

    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        string text = value?.ToString();

        if (string.IsNullOrWhiteSpace(text))
        {
            return ValidationResult.Success;
        }

        foreach (var word in _words)
        {
            if (text.Contains(
                word,
                StringComparison.OrdinalIgnoreCase))
            {
                return new ValidationResult(
                    $"The word '{word}' is not allowed.");
            }
        }

        return ValidationResult.Success;
    }
}
```

---

## Usage

```csharp
public class FeedbackDto
{
    [RestrictedWords(
        "badword1",
        "badword2")]
    public string Comments { get; set; }
}
```

---

# Generic Whitelist Attribute

Reusable whitelist validation.

```csharp
using System.ComponentModel.DataAnnotations;

public class WhitelistAttribute
    : ValidationAttribute
{
    private readonly string[] _allowedValues;

    public WhitelistAttribute(
        params string[] allowedValues)
    {
        _allowedValues = allowedValues;
    }

    protected override ValidationResult IsValid(
        object value,
        ValidationContext validationContext)
    {
        if (value == null)
        {
            return ValidationResult.Success;
        }

        string input = value.ToString();

        if (_allowedValues.Contains(
            input,
            StringComparer.OrdinalIgnoreCase))
        {
            return ValidationResult.Success;
        }

        return new ValidationResult(
            $"Allowed values are: {string.Join(", ", _allowedValues)}");
    }
}
```

---

## Usage

```csharp
public class DoctorDto
{
    [Whitelist(
        "Cardiology",
        "Neurology",
        "Dermatology")]
    public string Specialization { get; set; }
}
```

---

# Using Database-Based Blacklists

For dynamic blacklists:

```text
Blocked Emails
Blocked Domains
Blocked License Numbers
Blocked Hospitals
```

A custom Data Annotation is usually not ideal because it requires database access.

Instead, use:

```text
FluentValidation
Service Layer Validation
CQRS Validation Pipeline
```

Example:

```csharp
RuleFor(x => x.Email)
    .MustAsync(async (email, cancellationToken) =>
    {
        return !await _dbContext.BlockedEmails
            .AnyAsync(x => x.Email == email);
    })
    .WithMessage("Email is blocked.");
```

---

# Data Annotation vs FluentValidation

| Feature              | Data Annotation | FluentValidation |
| -------------------- | --------------- | ---------------- |
| Static Blacklist     | ✅               | ✅                |
| Static Whitelist     | ✅               | ✅                |
| Database Lookup      | ❌ Difficult     | ✅ Easy           |
| Dependency Injection | ⚠️ Limited      | ✅ Full Support   |
| Complex Rules        | ❌ Limited       | ✅ Excellent      |
| CQRS Compatible      | ⚠️ Limited      | ✅ Excellent      |

---


In a Clean Architecture + CQRS + MediatR application, custom Data Annotation attributes work well for **static whitelist/blacklist validation**, while **FluentValidation** is the preferred approach for dynamic rules that depend on database queries or external services.
