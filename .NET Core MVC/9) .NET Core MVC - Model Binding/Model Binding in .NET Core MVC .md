## What is Model Binding in ASP.NET Core?

**Model Binding** is the process by which ASP.NET Core automatically maps data from an HTTP request (Route, Query String, Form Data, Headers, Body, etc.) to action method parameters or model objects.

Instead of manually extracting values from `HttpRequest`, ASP.NET Core does it for you.

---

## Example Without Model Binding

```csharp
[HttpPost]
public IActionResult Create()
{
    var name = Request.Form["Name"];
    var age = Convert.ToInt32(Request.Form["Age"]);

    return Ok();
}
```

This works, but becomes difficult to maintain.

---

## Example With Model Binding

```csharp
public class UserDto
{
    public string Name { get; set; }
    public int Age { get; set; }
}

[HttpPost]
public IActionResult Create(UserDto model)
{
    return Ok(model);
}
```

Request:

```json
{
    "name": "Rohit",
    "age": 25
}
```

ASP.NET Core automatically creates a `UserDto` object and populates it.

---

# Sources of Data for Model Binding

ASP.NET Core can bind data from:

| Source         | Example              |
| -------------- | -------------------- |
| Route Values   | `/users/5`           |
| Query String   | `/users?id=5`        |
| Form Data      | HTML Form            |
| Request Body   | JSON/XML             |
| Headers        | Authorization Header |
| Uploaded Files | IFormFile            |

---

# 1. Route Binding

Route:

```csharp
[HttpGet("{id}")]
public IActionResult GetUser(int id)
{
    return Ok(id);
}
```

Request:

```http
GET /api/users/5
```

Output:

```json
5
```

`id` is automatically bound from the route.

---

# 2. Query String Binding

```csharp
[HttpGet]
public IActionResult Search(string name)
{
    return Ok(name);
}
```

Request:

```http
GET /api/users?name=rohit
```

Output:

```json
"rohit"
```

---

# 3. Form Data Binding

HTML Form:

```html
<form method="post">
    <input name="Name" />
    <input name="Age" />
</form>
```

Controller:

```csharp
[HttpPost]
public IActionResult Save(UserDto model)
{
    return Ok(model);
}
```

---

# 4. Body Binding (JSON)

Most common in Web APIs.

```csharp
[HttpPost]
public IActionResult Create(UserDto model)
{
    return Ok(model);
}
```

Request Body:

```json
{
  "name":"Rohit",
  "age":25
}
```

ASP.NET Core uses JSON serialization to populate the object.

---

# Binding Attributes

These attributes tell ASP.NET Core exactly where to get data from.

---

## [FromRoute]

```csharp
[HttpGet("{id}")]
public IActionResult Get([FromRoute] int id)
{
    return Ok(id);
}
```

Route:

```http
/api/users/10
```

Result:

```csharp
id = 10
```

---

## [FromQuery]

```csharp
[HttpGet]
public IActionResult Search([FromQuery] string city)
{
    return Ok(city);
}
```

Request:

```http
/api/users?city=Mumbai
```

Result:

```csharp
city = "Mumbai"
```

---

## [FromBody]

```csharp
[HttpPost]
public IActionResult Create([FromBody] UserDto user)
{
    return Ok(user);
}
```

Request:

```json
{
  "name":"Rohit",
  "age":25
}
```

---

## [FromForm]

```csharp
[HttpPost]
public IActionResult Upload([FromForm] UserDto user)
{
    return Ok(user);
}
```

Used for:

* Form submissions
* File uploads

---

## [FromHeader]

```csharp
[HttpGet]
public IActionResult GetToken(
    [FromHeader(Name = "Authorization")] string token)
{
    return Ok(token);
}
```

Header:

```http
Authorization: Bearer xyz123
```

---

## [FromServices]

Inject service directly into action.

```csharp
public IActionResult Get(
    [FromServices] ILogger<UserController> logger)
{
    return Ok();
}
```

Equivalent to constructor injection, but scoped to one action.

---

# Complex Type Binding

Suppose:

```csharp
public class RegisterDoctorDto
{
    public string Name { get; set; }
    public string Specialization { get; set; }
    public string Email { get; set; }
}
```

Controller:

```csharp
[HttpPost]
public IActionResult Register(RegisterDoctorDto dto)
{
    return Ok(dto);
}
```

Request:

```json
{
  "name":"Dr Rohit",
  "specialization":"Cardiology",
  "email":"rohit@test.com"
}
```

ASP.NET Core automatically maps all properties.

---

# Model Binding + Validation

```csharp
public class RegisterDoctorDto
{
    [Required]
    public string Name { get; set; }

    [EmailAddress]
    public string Email { get; set; }
}
```

Controller:

```csharp
[HttpPost]
public IActionResult Register(RegisterDoctorDto dto)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    return Ok(dto);
}
```

If validation fails:

```json
{
  "Email": [
    "The Email field is not a valid e-mail address."
  ]
}
```

---

# Model Binding in Your SmartHealthcare Project

Examples you'll use frequently:

### Login API

```csharp
[HttpPost("login")]
public async Task<IActionResult> Login(
    [FromBody] LoginCommand command)
{
}
```

---

### Get Doctor by Id

```csharp
[HttpGet("{id}")]
public async Task<IActionResult> GetDoctor(
    [FromRoute] Guid id)
{
}
```

---

### Search Hospitals

```csharp
[HttpGet]
public async Task<IActionResult> Search(
    [FromQuery] string city)
{
}
```

---

### Upload Doctor Certificate

```csharp
[HttpPost]
public async Task<IActionResult> UploadCertificate(
    [FromForm] UploadCertificateDto dto)
{
}
```

```csharp
public class UploadCertificateDto
{
    public IFormFile File { get; set; }
}
```

---

# Model Binding Pipeline

When a request arrives:

```text
HTTP Request
      ↓
Routing
      ↓
Model Binder
      ↓
Validation
      ↓
Controller Action
```

Example:

```json
{
  "email":"rohit@test.com",
  "password":"123456"
}
```

↓

```csharp
LoginCommand command
```

↓

Validation

↓

Controller executes

---

### Interview Definition

> Model Binding is an ASP.NET Core feature that automatically maps data from an HTTP request (route values, query strings, form data, headers, or request body) to action method parameters and model objects, reducing manual request parsing and simplifying controller code.
