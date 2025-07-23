
# ViewModel in ASP.NET Core MVC

## What is a ViewModel?

A **ViewModel** in ASP.NET Core MVC is a class that contains properties specifically designed to be used by a view. It helps to shape the data in the way the view requires it and may combine multiple models or transform data.

> ViewModels are not tied to your database, unlike Entity Framework models.

---

## Why Use ViewModels?

- Encapsulates only the data required for the view
- Helps reduce over-posting security issues
- Keeps concerns separate between the database layer and the UI
- Allows combining multiple models into a single class
- Improves clarity and maintainability

---

## Example Scenario

You want to display student details along with course name and enrollment status.

---

## Step 1: Create Domain Models

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Course
{
    public int Id { get; set; }
    public string Title { get; set; }
}
```

---

## Step 2: Create a ViewModel

```csharp
public class StudentViewModel
{
    public string StudentName { get; set; }
    public string CourseTitle { get; set; }
    public bool IsEnrolled { get; set; }
}
```

---

## Step 3: Use ViewModel in Controller

```csharp
public class StudentController : Controller
{
    public IActionResult Details()
    {
        var studentVM = new StudentViewModel
        {
            StudentName = "John Doe",
            CourseTitle = "ASP.NET Core",
            IsEnrolled = true
        };

        return View(studentVM);
    }
}
```

---

## Step 4: Create the Razor View (`Details.cshtml`)

```razor
@model YourNamespace.Models.StudentViewModel

<h2>Student Info</h2>
<p><strong>Name:</strong> @Model.StudentName</p>
<p><strong>Course:</strong> @Model.CourseTitle</p>
<p><strong>Status:</strong> @(Model.IsEnrolled ? "Enrolled" : "Not Enrolled")</p>
```

> Replace `YourNamespace.Models.StudentViewModel` with your actual namespace.

---

## Complex ViewModel Example

```csharp
public class DashboardViewModel
{
    public Student StudentInfo { get; set; }
    public List<Course> AvailableCourses { get; set; }
}
```

This helps when you want to render multiple model types in a single view.

---

## Summary

- ViewModels are essential for shaping and managing data specifically for the view.
- They enhance security, maintainability, and separation of concerns.
- You can use them to combine and simplify data from multiple sources.

> Always tailor your ViewModel to the exact needs of your view for clarity and performance.
