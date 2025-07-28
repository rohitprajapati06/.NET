
# FileResult in ASP.NET Core MVC

## What is `FileResult`?

`FileResult` is an abstract base class in ASP.NET Core MVC used to return binary file content (like PDFs, images, or any downloadable files) in the HTTP response.

Derived types include:
- `FileContentResult`
- `FileStreamResult`
- `VirtualFileResult`
- `PhysicalFileResult`

---

## Namespace

```csharp
using Microsoft.AspNetCore.Mvc;
```

---

## Common Scenarios

- Download a PDF, image, or ZIP file
- Return dynamically generated files
- Stream large files to the client

---

## Example: Return a Byte Array

```csharp
public IActionResult DownloadPdf()
{
    byte[] fileBytes = System.IO.File.ReadAllBytes("wwwroot/files/sample.pdf");
    return File(fileBytes, "application/pdf", "sample.pdf");
}
```

---

## Example: Return a Stream

```csharp
public IActionResult DownloadLog()
{
    var stream = new FileStream("logs/log.txt", FileMode.Open);
    return File(stream, "text/plain", "log.txt");
}
```

---

## Example: Return a Physical File (Path)

```csharp
public IActionResult GetImage()
{
    string path = Path.Combine(Directory.GetCurrentDirectory(), "wwwroot/images/photo.jpg");
    return PhysicalFile(path, "image/jpeg", "photo.jpg");
}
```

---

## Content Types (MIME Types)

| File Type | MIME Type             |
|-----------|------------------------|
| PDF       | `application/pdf`     |
| JPEG      | `image/jpeg`          |
| PNG       | `image/png`           |
| TXT       | `text/plain`          |
| ZIP       | `application/zip`     |

---

## Return Types

| Return Type           | Description                                  |
|------------------------|----------------------------------------------|
| `FileContentResult`   | Returns file from byte array                 |
| `FileStreamResult`    | Returns file from a stream                   |
| `PhysicalFileResult`  | Returns file from physical file system path |
| `VirtualFileResult`   | Returns file from virtual path (e.g., wwwroot) |

---

## Summary

- `FileResult` is used to send files as responses in ASP.NET Core MVC.
- Supports returning from byte arrays, streams, or file paths.
- Proper MIME types help browsers handle downloads or display files.

> Use `FileResult` when you need to serve downloadable content or media files from your controller actions.
