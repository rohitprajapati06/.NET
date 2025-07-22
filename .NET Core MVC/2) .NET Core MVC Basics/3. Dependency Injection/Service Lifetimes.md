
# Singleton vs. Scoped vs. Transient Services in ASP.NET Core

In this article, we will discuss the **Singleton vs. Scoped vs. Transient Services** in an ASP.NET Core Application with real-time examples.

> Please read our previous article where we discussed [ASP.NET Core Dependency Injection with Examples](./DependencyInjection.md).

---

## Example to Understand Service Lifetimes

Let us understand the differences between Singleton, Scoped, and Transient Services with an example.

### Student Model

```csharp
namespace FirstCoreMVCWebApplication.Models
{
    public class Student
    {
        public int StudentId { get; set; }
        public string? Name { get; set; }
        public string? Branch { get; set; }
        public string? Section { get; set; }
        public string? Gender { get; set; }
    }
}
```

### IStudentRepository Interface

```csharp
namespace FirstCoreMVCWebApplication.Models
{
    public interface IStudentRepository
    {
        Student GetStudentById(int StudentId);
        List<Student> GetAllStudent();
    }
}
```

### StudentRepository

```csharp
namespace FirstCoreMVCWebApplication.Models
{
    public class StudentRepository : IStudentRepository
    {
        public StudentRepository()
        {
            string filePath = @"D:\MyProjects\FirstCoreMVCWebApplication\Log\Log.txt";
            string contentToWrite = $"StudentRepository Object Created: @{DateTime.Now.ToString()}";
            using (StreamWriter writer = new StreamWriter(filePath, true))
            {
                writer.WriteLine(contentToWrite);
            }
        }

        public List<Student> DataSource()
        {
            return new List<Student>()
            {
                new Student() { StudentId = 101, Name = "James", Branch = "CSE", Section = "A", Gender = "Male" },
                new Student() { StudentId = 102, Name = "Smith", Branch = "ETC", Section = "B", Gender = "Male" },
                new Student() { StudentId = 103, Name = "David", Branch = "CSE", Section = "A", Gender = "Male" },
                new Student() { StudentId = 104, Name = "Sara", Branch = "CSE", Section = "A", Gender = "Female" },
                new Student() { StudentId = 105, Name = "Pam", Branch = "ETC", Section = "B", Gender = "Female" }
            };
        }

        public Student GetStudentById(int StudentId)
        {
            return DataSource().FirstOrDefault(e => e.StudentId == StudentId) ?? new Student();
        }

        public List<Student> GetAllStudent()
        {
            return DataSource();
        }
    }
}
```

### SomeOtherService

```csharp
namespace FirstCoreMVCWebApplication.Models
{
    public class SomeOtherService
    {
        private readonly IStudentRepository? _repository = null;

        public SomeOtherService(IStudentRepository repository)
        {
            _repository = repository;
        }

        public void SomeMethod()
        {
            // Use StudentRepository service
        }
    }
}
```

### HomeController

```csharp
using FirstCoreMVCWebApplication.Models;
using Microsoft.AspNetCore.Mvc;

namespace FirstCoreMVCWebApplication.Controllers
{
    public class HomeController : Controller
    {
        private readonly IStudentRepository? _repository = null;
        private readonly SomeOtherService? _someOtherService = null;

        public HomeController(IStudentRepository repository, SomeOtherService someOtherService)
        {
            _repository = repository;
            _someOtherService = someOtherService;
        }

        public JsonResult Index()
        {
            List<Student>? allStudentDetails = _repository?.GetAllStudent();
            _someOtherService?.SomeMethod();
            return Json(allStudentDetails);
        }

        public JsonResult GetStudentDetails(int Id)
        {
            Student? studentDetails = _repository?.GetStudentById(Id);
            _someOtherService?.SomeMethod();
            return Json(studentDetails);
        }
    }
}
```

---

## Singleton Service

- **Definition**: One instance is created and shared throughout the application.
- **Use Case**: Configuration, caching, logging, shared memory.

```csharp
builder.Services.AddSingleton<IStudentRepository, StudentRepository>();
builder.Services.AddSingleton<SomeOtherService>();
```

- **Behavior**: Only one object is created even after multiple requests.

### When to Use:
- Shared state across the app
- Expensive to create and reusable objects

---

## Scoped Service

- **Definition**: One instance per request. Shared only within the same request.
- **Use Case**: EF Core `DbContext`

```csharp
builder.Services.AddScoped<IStudentRepository, StudentRepository>();
builder.Services.AddScoped<SomeOtherService>();
```

- **Behavior**: Two objects created across two requests

### When to Use:
- Maintain state within request
- Unit-of-work patterns

---

## Transient Service

- **Definition**: A new instance each time it’s requested.
- **Use Case**: Lightweight, stateless operations

```csharp
builder.Services.AddTransient<IStudentRepository, StudentRepository>();
builder.Services.AddTransient<SomeOtherService>();
```

- **Behavior**: Four objects created across two requests

### When to Use:
- Stateless services
- Simple operations

---

## Conclusion

| Lifetime   | Description                                | Usage Examples                        |
|------------|--------------------------------------------|----------------------------------------|
| Singleton  | One instance for the whole application     | Caching, config, logging               |
| Scoped     | One instance per request                   | EF Core DbContext, unit of work        |
| Transient  | New instance every time requested          | Lightweight services, calculators      |

Choosing the correct service lifetime improves performance and application maintainability.

---

> In the next article, we will discuss creating an ASP.NET Core Application using the MVC Template with Examples.
