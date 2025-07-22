# ASP.NET Core Dependency Injection

## 🧠 What is Dependency Injection?

**Dependency Injection (DI)** is a design pattern that allows a class to receive its dependencies from external sources rather than creating them internally. It promotes **loose coupling**, **testability**, and **separation of concerns**.

---

## 🚨 Why Do We Need It?

Without DI, classes directly instantiate their dependencies, leading to **tight coupling**. This can make code hard to:
- Maintain
- Extend
- Unit Test

Example of **tight coupling**:
```csharp
public class HomeController : Controller
{
    public JsonResult Index()
    {
        StudentRepository repository = new StudentRepository();
        var students = repository.GetAllStudent();
        return Json(students);
    }
}
```

---

## ✅ Benefits of Dependency Injection

- 🔁 **Loose Coupling**
- 🧪 **Improved Testability** (Mocking dependencies becomes easy)
- 🔧 **Better Maintainability**
- 🧩 **Reusable Components**
- ⚙️ **Automatic Lifetime Management** via IoC Container

---

## 🧰 How Does ASP.NET Core Support DI?

ASP.NET Core has **built-in support** for dependency injection via an **IoC (Inversion of Control)** container.

### Basic Steps:
1. **Create Interface**
2. **Implement Service**
3. **Register with DI Container**
4. **Inject via Constructor / Action Method**

---

## 📦 Creating Model and Repository

### `Student.cs`
```csharp
public class Student
{
    public int StudentId { get; set; }
    public string? Name { get; set; }
    public string? Branch { get; set; }
    public string? Section { get; set; }
    public string? Gender { get; set; }
}
```

### `IStudentRepository.cs`
```csharp
public interface IStudentRepository
{
    Student GetStudentById(int id);
    List<Student> GetAllStudent();
}
```

### `StudentRepository.cs`
```csharp
public class StudentRepository : IStudentRepository
{
    public List<Student> GetAllStudent() => new List<Student>
    {
        new Student { StudentId = 101, Name = "James", Branch = "CSE", Section = "A", Gender = "Male" },
        // more students...
    };

    public Student GetStudentById(int id) =>
        GetAllStudent().FirstOrDefault(s => s.StudentId == id) ?? new Student();
}
```

---

## 🔧 Registering the Service in `Program.cs`

### Register MVC and the Repository
```csharp
var builder = WebApplication.CreateBuilder(args);

// Framework Service
builder.Services.AddControllersWithViews();

// Application Service (Choose lifetime)
builder.Services.AddSingleton<IStudentRepository, StudentRepository>();
// Or:
// builder.Services.AddScoped<IStudentRepository, StudentRepository>();
// builder.Services.AddTransient<IStudentRepository, StudentRepository>();

var app = builder.Build();
app.UseRouting();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

---

## 📥 Injecting Dependencies

### 1️⃣ Constructor Injection (Recommended)
```csharp
public class HomeController : Controller
{
    private readonly IStudentRepository _repository;

    public HomeController(IStudentRepository repository)
    {
        _repository = repository;
    }

    public JsonResult Index()
    {
        return Json(_repository.GetAllStudent());
    }
}
```

### 2️⃣ Action Method Injection
```csharp
public JsonResult Index([FromServices] IStudentRepository repository)
{
    return Json(repository.GetAllStudent());
}
```

### 3️⃣ Manual Retrieval (Not Recommended)
```csharp
public JsonResult Index()
{
    var repository = (IStudentRepository?)HttpContext.RequestServices.GetService(typeof(IStudentRepository));
    return Json(repository?.GetAllStudent());
}
```

---

## ⏳ Service Lifetimes in ASP.NET Core

| Lifetime   | Description |
|------------|-------------|
| `Singleton` | One instance for the entire app lifetime |
| `Scoped`    | One instance per HTTP request |
| `Transient` | A new instance each time requested |

```csharp
// Singleton
builder.Services.AddSingleton<IStudentRepository, StudentRepository>();

// Scoped
builder.Services.AddScoped<IStudentRepository, StudentRepository>();

// Transient
builder.Services.AddTransient<IStudentRepository, StudentRepository>();
```

---

## 🧩 [FromServices] Attribute

Used to inject dependencies **into action methods**.

```csharp
public IActionResult Index([FromServices] IStudentRepository repo)
{
    var students = repo.GetAllStudent();
    return View(students);
}
```

---

## ❌ Property Injection?

ASP.NET Core’s built-in DI container **does not support property injection**. You must use third-party containers (e.g., Autofac) for that.

---

## 🏆 Advantages of DI in ASP.NET Core

- Improves **testability** with mock services
- Supports **interface-based design**
- Makes services **easy to swap**
- **Centralizes configuration** of app services
- **Simplifies controller code**

---

## 🧪 Unit Testing with DI

Use mocking libraries (like Moq) to test your controllers without using real service implementations:
```csharp
var mockRepo = new Mock<IStudentRepository>();
mockRepo.Setup(repo => repo.GetAllStudent()).Returns(new List<Student> { ... });

var controller = new HomeController(mockRepo.Object);
```

---

## 📚 Further Reading

- [Microsoft Docs – Dependency Injection](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection)
- [Service Lifetimes](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#service-lifetimes)
