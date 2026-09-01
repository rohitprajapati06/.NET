# AutoMapper in ASP.NET Core Web API

**AutoMapper** is a library used to automatically map objects from one class to another, mainly between **Entities and DTOs**.

Instead of manually writing:

```csharp
var dto = new PatientDto
{
    Id = patient.Id,
    Name = patient.Name,
    Email = patient.Email
};
```

AutoMapper allows:

```csharp
var dto = _mapper.Map<PatientDto>(patient);
```

---

## 1. Why do we need AutoMapper?

In a typical ASP.NET Core Web API, we usually have:

```text
Database
   ↓
Entity
   ↓
Service
   ↓
DTO
   ↓
Controller
   ↓
API Response
```

For example:

### Entity

```csharp
public class Patient
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public string Password { get; set; }
}
```

### DTO

```csharp
public class PatientDto
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}
```

Notice that `Password` should **not** be returned by the API.

Without AutoMapper, we manually map:

```csharp
var dto = new PatientDto
{
    Id = patient.Id,
    Name = patient.Name,
    Email = patient.Email
};
```

With AutoMapper:

```csharp
var dto = _mapper.Map<PatientDto>(patient);
```

AutoMapper automatically maps properties with matching names.

---

# 2. Install AutoMapper

Install the NuGet package:

```bash
dotnet add package AutoMapper
```

Depending on your project/version, you may also need the ASP.NET Core integration package.

---

# 3. Create a Mapping Profile

Create a folder such as:

```text
Application
 └── Mapping
      └── MappingProfile.cs
```

```csharp
using AutoMapper;

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Patient, PatientDto>();
    }
}
```

This tells AutoMapper:

> Whenever I ask you to map `Patient` → `PatientDto`, automatically map the matching properties.

---

# 4. Register AutoMapper

In `Program.cs`:

```csharp
builder.Services.AddAutoMapper(typeof(MappingProfile));
```

Now AutoMapper is available through Dependency Injection.

---

# 5. Inject `IMapper`

For example, in a service:

```csharp
public class PatientService
{
    private readonly IMapper _mapper;

    public PatientService(IMapper mapper)
    {
        _mapper = mapper;
    }
}
```

Then:

```csharp
var patientDto = _mapper.Map<PatientDto>(patient);
```

---

# 6. Mapping in Both Directions

Usually you'll need mapping in both directions:

```csharp
CreateMap<Patient, PatientDto>();
CreateMap<PatientDto, Patient>();
```

Then:

```csharp
PatientDto dto = _mapper.Map<PatientDto>(patient);
```

and:

```csharp
Patient patient = _mapper.Map<Patient>(dto);
```

However, in real applications, you often **shouldn't map DTOs directly back to entities** without considering fields such as IDs, audit fields, relationships, etc.

---

# 7. Mapping Different Property Names

Suppose:

```csharp
public class Patient
{
    public int Id { get; set; }
    public string FullName { get; set; }
}
```

DTO:

```csharp
public class PatientDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

AutoMapper won't automatically know that `FullName` → `Name`.

Configure it:

```csharp
CreateMap<Patient, PatientDto>()
    .ForMember(
        dest => dest.Name,
        opt => opt.MapFrom(src => src.FullName)
    );
```

Now:

```text
Patient.FullName
       ↓
PatientDto.Name
```

---

# 8. Mapping Collections

Suppose you retrieve:

```csharp
List<Patient> patients;
```

You can map the entire collection:

```csharp
var patientDtos = _mapper.Map<List<PatientDto>>(patients);
```

No `foreach` required.

---

# 9. AutoMapper in a Controller

Example:

```csharp
[ApiController]
[Route("api/[controller]")]
public class PatientsController : ControllerBase
{
    private readonly IMapper _mapper;
    private readonly IPatientService _patientService;

    public PatientsController(
        IMapper mapper,
        IPatientService patientService)
    {
        _mapper = mapper;
        _patientService = patientService;
    }

    [HttpGet]
    public async Task<IActionResult> GetPatients()
    {
        var patients = await _patientService.GetPatients();

        var result = _mapper.Map<List<PatientDto>>(patients);

        return Ok(result);
    }
}
```

However, in a **Clean Architecture / layered enterprise application**, I'd generally prefer keeping mapping outside the controller, typically in the **Application/Service layer**, so the controller remains thin.

---

# 10. AutoMapper vs Manual Mapping

| Feature                 | Manual Mapping        | AutoMapper                |
| ----------------------- | --------------------- | ------------------------- |
| Code required           | More                  | Less                      |
| Setup                   | None                  | Mapping profiles required |
| Simple mappings         | Very clear            | Very convenient           |
| Complex mappings        | More control          | Configuration required    |
| Debugging               | Easier                | Can be less obvious       |
| Large number of DTOs    | Repetitive            | Reduces boilerplate       |
| Performance             | Slightly better       | Small mapping overhead    |
| Enterprise applications | Common                | Common                    |
| Best for                | Small/simple projects | Many DTO/entity mappings  |

---

## 11. Important Interview Point

AutoMapper **doesn't eliminate the need for DTOs**.

The purpose is:

```text
Entity ──────── Manual mapping ────────> DTO
Entity ──────── AutoMapper ────────────> DTO
```

DTOs are still important because they:

* Prevent exposing database entities directly
* Control what the API returns
* Prevent sensitive fields from being exposed
* Decouple API contracts from database models
* Allow different request/response models

---

## 12. Typical Enterprise Structure

For your .NET Web API project, you can think of it like:

```text
SmartHealthcare
│
├── Domain
│   └── Entities
│       └── Patient.cs
│
├── Application
│   ├── DTOs
│   │   └── PatientDto.cs
│   │
│   ├── Mapping
│   │   └── MappingProfile.cs
│   │
│   └── Services
│       └── PatientService.cs
│
├── Infrastructure
│   └── Persistence
│
└── API
    └── Controllers
        └── PatientsController.cs
```

The basic flow becomes:

```text
Database
   ↓
Patient Entity
   ↓
Service
   ↓
AutoMapper
   ↓
PatientDto
   ↓
Controller
   ↓
HTTP Response
```

### Key things to remember

**AutoMapper = object-to-object mapping library.**

Most commonly:

```csharp
CreateMap<Patient, PatientDto>();
```

then:

```csharp
_mapper.Map<PatientDto>(patient);
```

For interviews, understand **mapping profiles, `IMapper`, `CreateMap`, `ForMember`, collection mapping, reverse mapping, and Entity ↔ DTO separation**.
