# Quick Ref: Entity Framework Core

## Instalación básica
```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

## Ejemplo de contexto
```csharp
public class AppDbContext : DbContext
{
    public DbSet<Producto> Productos { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Server=localhost;Database=MiDb;User Id=sa;Password=TuPassword;");
    }
}
```

## Enlaces oficiales
- EF Core documentation: https://learn.microsoft.com/ef/core/
- DbContext: https://learn.microsoft.com/ef/core/dbcontext-configuration/
- Migrations: https://learn.microsoft.com/ef/core/managing-schemas/migrations/
