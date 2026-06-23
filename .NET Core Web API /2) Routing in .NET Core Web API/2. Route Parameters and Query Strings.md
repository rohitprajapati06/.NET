# Route Parameters vs Query Strings in ASP.NET Core Web API

Both are used to pass data from the client to an API endpoint, but they serve different purposes.

---

# 1. Route Parameters

Route parameters are part of the URL path itself.

They usually identify a **specific resource**.

### Example

```csharp
[HttpGet("{id}")]
public IActionResult GetHospital(Guid id)
{
    return Ok(id);
}
```

Request:

```http
GET /api/hospitals/5d7f3f3f-1234-4567-8901-abcdef123456
```

ASP.NET Core extracts the value from the URL and binds it to `id`.

```text
id = 5d7f3f3f-1234-4567-8901-abcdef123456
```

---

## Multiple Route Parameters

```csharp
[HttpGet("{hospitalId}/doctors/{doctorId}")]
public IActionResult GetDoctor(Guid hospitalId, Guid doctorId)
{
    return Ok();
}
```

Request:

```http
GET /api/hospitals/1/doctors/2
```

Values:

```text
hospitalId = 1
doctorId = 2
```

---

## Optional Route Parameters

```csharp
[HttpGet("{id?}")]
public IActionResult Get(Guid? id)
{
    return Ok();
}
```

Valid requests:

```http
GET /api/hospitals

GET /api/hospitals/1
```

---

## Route Constraints

```csharp
[HttpGet("{id:guid}")]
public IActionResult Get(Guid id)
{
}
```

Only GUID values are accepted.

---

# 2. Query Strings

Query strings appear after a `?` in the URL.

They are commonly used for:

* Filtering
* Searching
* Sorting
* Pagination

---

### Example

Controller:

```csharp
[HttpGet]
public IActionResult GetHospitals(string city)
{
    return Ok(city);
}
```

Request:

```http
GET /api/hospitals?city=Mumbai
```

Value:

```text
city = Mumbai
```

---

## Multiple Query Parameters

```csharp
[HttpGet]
public IActionResult GetHospitals(
    string city,
    string state)
{
    return Ok();
}
```

Request:

```http
GET /api/hospitals?city=Mumbai&state=Maharashtra
```

Values:

```text
city = Mumbai
state = Maharashtra
```

---

## Using `[FromQuery]`

Being explicit improves readability.

```csharp
[HttpGet]
public IActionResult Search(
    [FromQuery] string city,
    [FromQuery] int page = 1)
{
    return Ok();
}
```

Request:

```http
GET /api/hospitals?city=Mumbai&page=2
```

---

# Combining Route Parameters and Query Strings

You can use both together.

```csharp
[HttpGet("{hospitalId}")]
public IActionResult GetAppointments(
    Guid hospitalId,
    [FromQuery] DateTime date)
{
    return Ok();
}
```

Request:

```http
GET /api/hospitals/1/appointments?date=2026-06-23
```

Values:

```text
hospitalId = 1
date = 2026-06-23
```

---

# Example from Your SmartHealthcare Project

Suppose you want to get appointments for a hospital with pagination.

```csharp
[HttpGet("{hospitalId}/appointments")]
public async Task<IActionResult> GetAppointments(
    Guid hospitalId,
    [FromQuery] int page = 1,
    [FromQuery] int pageSize = 10)
{
    var query = new GetAppointmentsQuery(
        hospitalId,
        page,
        pageSize);

    return Ok(await mediator.Send(query));
}
```

Request:

```http
GET /api/hospitals/3fa85f64-5717-4562-b3fc-2c963f66afa6/appointments?page=2&pageSize=20
```

Extracted values:

```text
hospitalId = 3fa85f64-5717-4562-b3fc-2c963f66afa6
page = 2
pageSize = 20
```

---

# Route Parameters vs Query Strings

| Aspect     | Route Parameters        | Query Strings              |
| ---------- | ----------------------- | -------------------------- |
| Location   | URL path                | After `?`                  |
| Purpose    | Identify a resource     | Filter or modify results   |
| Required   | Usually yes             | Usually optional           |
| Example    | `/api/hospitals/1`      | `/api/hospitals?page=1`    |
| REST Style | Resource identification | Search, filter, pagination |

---

# REST API Design Guidelines

Use **Route Parameters** for resource identifiers:

```http
GET /api/hospitals/1
GET /api/doctors/10
GET /api/patients/25
```

Use **Query Strings** for filtering and paging:

```http
GET /api/hospitals?city=Mumbai
GET /api/doctors?specialization=Cardiology
GET /api/appointments?page=1&pageSize=20
```

A good rule of thumb is:

> If the value identifies **which resource** you want, use a **route parameter**.
> If the value changes **how the results are returned**, use a **query string**.
