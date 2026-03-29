---
description: Rules cho dự án C# / .NET — naming conventions, async/await, dependency injection, Entity Framework, API design.
author: TaiVo-AI
version: 1.0
tags: ["csharp", "dotnet", "aspnetcore", "entityframework", "backend"]
globs: ["**/*.cs", "**/*.csproj", "**/*.sln"]
---

# C# / .NET Guidelines

## Naming Conventions

| Loại | Convention | Ví dụ |
|------|-----------|-------|
| Class, Interface, Enum | PascalCase | `UserService`, `IRepository<T>` |
| Method, Property | PascalCase | `GetUserById()`, `FirstName` |
| Private field | `_camelCase` | `_userRepository` |
| Local variable, param | camelCase | `userId`, `isActive` |
| Constant | PascalCase | `MaxRetryCount` |
| Interface | Prefix `I` | `IUserService`, `ILogger` |
| Async method | Suffix `Async` | `GetUserAsync()` |

## Cấu trúc Project (Clean Architecture)

```
Solution/
├── Domain/                  # Entities, Value Objects, Domain Events
│   ├── Entities/
│   ├── Interfaces/
│   └── Exceptions/
├── Application/             # Use Cases, DTOs, Validators
│   ├── Features/
│   │   └── Users/
│   │       ├── Commands/
│   │       └── Queries/
│   └── Common/
├── Infrastructure/          # EF Core, External Services
│   ├── Persistence/
│   └── Services/
└── API/                     # Controllers, Middleware
    ├── Controllers/
    └── Middleware/
```

## Async/Await — Quy tắc bắt buộc

```csharp
// ✅ ĐÚNG — async tất cả I/O operations
public async Task<UserDto> GetUserAsync(int id, CancellationToken ct = default)
{
    var user = await _repository.GetByIdAsync(id, ct)
        ?? throw new NotFoundException(nameof(User), id);
    return _mapper.Map<UserDto>(user);
}

// ✅ ĐÚNG — ConfigureAwait(false) trong library code
var result = await _httpClient.GetAsync(url).ConfigureAwait(false);

// ❌ SAI — Blocking async code gây deadlock
var user = _repository.GetByIdAsync(id).Result;    // BANNED
var user = _repository.GetByIdAsync(id).GetAwaiter().GetResult(); // BANNED

// ❌ SAI — async void (không catch được exception)
public async void HandleEvent() { }  // BANNED — trừ event handlers
```

## Dependency Injection

```csharp
// ✅ ĐÚNG — Constructor injection
public class UserService : IUserService
{
    private readonly IUserRepository _repository;
    private readonly ILogger<UserService> _logger;

    public UserService(IUserRepository repository, ILogger<UserService> logger)
    {
        _repository = repository;
        _logger = logger;
    }
}

// ✅ ĐÚNG — Đăng ký DI theo vòng đời đúng
services.AddScoped<IUserService, UserService>();      // Per HTTP request
services.AddTransient<IEmailSender, SmtpEmailSender>(); // New instance mỗi lần
services.AddSingleton<ICacheService, RedisCacheService>(); // 1 instance toàn app

// ❌ SAI — Service Locator pattern
var service = serviceProvider.GetService<IUserService>(); // Tránh trong business code
```

## Entity Framework Core

```csharp
// ✅ ĐÚNG — Async queries với CancellationToken
var users = await _context.Users
    .Where(u => u.IsActive && u.Department == dept)
    .Select(u => new UserDto { Id = u.Id, Name = u.FullName })
    .AsNoTracking()  // Read-only queries
    .ToListAsync(cancellationToken);

// ✅ ĐÚNG — Transactions cho multi-table updates
await using var transaction = await _context.Database.BeginTransactionAsync(ct);
try {
    _context.Orders.Add(order);
    _context.Inventory.Update(item);
    await _context.SaveChangesAsync(ct);
    await transaction.CommitAsync(ct);
} catch {
    await transaction.RollbackAsync(ct);
    throw;
}

// ❌ SAI — N+1 query problem
foreach (var order in orders)
    await _context.Entry(order).Reference(o => o.Customer).LoadAsync(); // BANNED
// ✅ Thay bằng:
var orders = await _context.Orders.Include(o => o.Customer).ToListAsync(ct);
```

## ASP.NET Core API

```csharp
// ✅ ĐÚNG — Controller với proper response types
[ApiController]
[Route("api/v1/[controller]")]
public class UsersController : ControllerBase
{
    [HttpGet("{id:int}")]
    [ProducesResponseType<UserDto>(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<ActionResult<UserDto>> GetUser(int id, CancellationToken ct)
    {
        var user = await _userService.GetUserAsync(id, ct);
        return Ok(user);
    }
}
```

## Exception Handling

```csharp
// Domain exceptions
public class NotFoundException : Exception
{
    public NotFoundException(string name, object key)
        : base($"Entity '{name}' with key '{key}' not found.") { }
}

// Global exception middleware — KHÔNG try-catch trong từng controller
app.UseExceptionHandler(errorApp => errorApp.Run(async context => {
    var ex = context.Features.Get<IExceptionHandlerFeature>()?.Error;
    var (status, message) = ex switch {
        NotFoundException => (404, ex.Message),
        ValidationException => (400, ex.Message),
        _ => (500, "An unexpected error occurred.")
    };
    context.Response.StatusCode = status;
    await context.Response.WriteAsJsonAsync(new { error = message });
}));
```

## Không được làm

- KHÔNG `async void` trừ event handlers
- KHÔNG dùng `.Result` hoặc `.Wait()` trên Task
- KHÔNG catch `Exception` chung chung mà không rethrow
- KHÔNG hardcode connection strings — dùng `IConfiguration`
- KHÔNG dùng `var` khi type không rõ ràng từ context
- KHÔNG bỏ qua `CancellationToken` trong public methods
