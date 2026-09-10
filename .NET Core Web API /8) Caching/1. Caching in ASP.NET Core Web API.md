# Caching in ASP.NET Core Web API

**Caching** is the technique of storing frequently accessed data temporarily so that future requests can be served faster without repeatedly accessing the database or performing expensive operations.

### Why use Caching?

* ⚡ Improves API performance
* 🗄️ Reduces database calls
* 📉 Reduces server load
* 🚀 Improves response time

### Types of Caching in ASP.NET Core

| Type                    | Description                                                              |
| ----------------------- | ------------------------------------------------------------------------ |
| **In-Memory Caching**   | Stores data in the application's server memory                           |
| **Distributed Caching** | Stores data in a shared cache such as Redis, useful for multiple servers |
| **Response Caching**    | Caches complete HTTP responses                                           |
| **Output Caching**      | Caches generated responses on the server based on configured policies    |

### 1. In-Memory Cache

Uses `IMemoryCache`.

```csharp
builder.Services.AddMemoryCache();
```

Example:

```csharp
private readonly IMemoryCache _cache;

public async Task<IActionResult> GetProducts()
{
    if (!_cache.TryGetValue("products", out List<Product>? products))
    {
        products = await _context.Products.ToListAsync();

        _cache.Set("products", products, TimeSpan.FromMinutes(10));
    }

    return Ok(products);
}
```

The first request gets data from the database; subsequent requests can get it from memory until the cache expires.

### 2. Distributed Cache

Uses `IDistributedCache` and is suitable for **multiple API instances**.

Common implementation:

**Redis**

```csharp
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = "localhost:6379";
});
```

### 3. Response/Output Caching

Instead of manually caching data, ASP.NET Core can cache the **HTTP response**.

Output caching is commonly configured using:

```csharp
builder.Services.AddOutputCache();

app.UseOutputCache();
```

Then:

```csharp
[OutputCache(Duration = 60)]
[HttpGet]
public IActionResult GetProducts()
{
    return Ok(_context.Products.ToList());
}
```

The response can be served from the cache for 60 seconds.

### Important Difference

**Data caching** → caches data, e.g., products retrieved from DB.

**Response/Output caching** → caches the complete API response.

