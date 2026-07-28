# Quick Ref: ASP.NET Core 10

## Conceptos clave
- Middleware: componentes que procesan solicitudes HTTP.
- Routing: definición de rutas mediante atributos o endpoints.
- Dependency Injection: inyección de servicios para desacoplar componentes.
- Minimal APIs: enfoque simple para crear endpoints rápidos.

## Ejemplo mínimo
```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hola desde ASP.NET Core 10");

app.Run();
```

## Enlaces oficiales
- ASP.NET Core overview: https://learn.microsoft.com/aspnet/core/
- Minimal APIs: https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis
- Dependency Injection: https://learn.microsoft.com/aspnet/core/fundamentals/dependency-injection
