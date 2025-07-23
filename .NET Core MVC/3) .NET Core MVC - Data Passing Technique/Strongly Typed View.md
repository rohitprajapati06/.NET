
# Strongly Typed Views in ASP.NET Core MVC

## What is a Strongly Typed View?

A **strongly typed view** in ASP.NET Core is a Razor view that is bound to a specific model class. This allows the view to access properties of the model using IntelliSense and compile-time checking.

---

## Why Use Strongly Typed Views?

- **Compile-time type checking**
- **IntelliSense support** in Razor views
- Simplifies data binding between controller and view
- Reduces runtime errors

---

## Step-by-Step: Creating a Strongly Typed View

### Step 1: Define a Model

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Course { get; set; }
}
```

---

### Step 2: Pass Model from Controller

```csharp
public class StudentController : Controller
{
    public IActionResult Details()
    {
        var student = new Student
        {
            Id = 1,
            Name = "John Doe",
            Course = "Computer Science"
        };

        return View(student); // Passing the model to the view
    }
}
```

---

### Step 3: Create Strongly Typed View

Create a Razor view named `Details.cshtml` inside the `Views/Student` folder.

```razor
@model YourNamespace.Models.Student

<h2>Student Details</h2>

<p><strong>ID:</strong> @Model.Id</p>
<p><strong>Name:</strong> @Model.Name</p>
<p><strong>Course:</strong> @Model.Course</p>
```

> Replace `YourNamespace.Models.Student` with the actual namespace of your model.

---

## Tips

- Use `@model` directive at the top of the Razor view to bind the model
- Use `@Model.PropertyName` to access model data
- You can also bind collections of models using `IEnumerable<ModelType>`

---

## Example: Binding a List of Students

### Controller

```csharp
public IActionResult Index()
{
    var students = new List<Student>
    {
        new Student { Id = 1, Name = "John", Course = "CS" },
        new Student { Id = 2, Name = "Jane", Course = "Math" }
    };

    return View(students);
}
```

### View (`Index.cshtml`)

```razor
@model IEnumerable<YourNamespace.Models.Student>

<h2>Student List</h2>
<ul>
@foreach (var student in Model)
{
    <li>@student.Name - @student.Course</li>
}
</ul>
```

---

## Summary

- Strongly typed views improve the robustness of your Razor views.
- They provide compile-time checking and IntelliSense.
- Always ensure the model passed from the controller matches the one declared with `@model`.

> Strongly typed views are a best practice in ASP.NET Core MVC for rendering dynamic data.

