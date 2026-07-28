# Guía de despliegue en Ubuntu

## Objetivo
Desplegar una aplicación ASP.NET Core 10 en un servidor Ubuntu con PostgreSQL y NGINX.

## Pasos rápidos

### 1. Preparar el servidor
Actualiza el sistema e instala los componentes base:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl gnupg ca-certificates lsb-release ufw
```

Instala el runtime de .NET 10 usando los repositorios de Ubuntu/Canonical:

```bash
sudo apt update
sudo apt install -y dotnet-runtime-10.0
```

Si tu versión de Ubuntu no lo ofrece de forma inmediata, puedes verificar con:

```bash
apt search dotnet-runtime-10.0
```

Instala PostgreSQL y NGINX:

```bash
sudo apt install -y postgresql postgresql-contrib nginx
```

Enlaces oficiales de referencia:
- Ubuntu / Canonical: https://ubuntu.com/server/docs/installing-dotnet
- PostgreSQL: https://www.postgresql.org/download/linux/ubuntu/
- NGINX: https://nginx.org/en/linux_packages.html

### 2. Configurar PostgreSQL
```bash
sudo -u postgres psql
```
```sql
CREATE ROLE appuser WITH LOGIN PASSWORD 'TuPasswordSegura';
CREATE DATABASE appdb OWNER appuser;
ALTER ROLE appuser WITH LOGIN;
\q
```

Ejemplo de archivo de configuración en el servidor, en /var/www/miapp/appsettings.Production.json:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=127.0.0.1;Port=5432;Database=appdb;Username=appuser;Password=TuPasswordSegura"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning"
    }
  }
}
```

Para usar esa conexión desde la aplicación Blazor, en Program.cs agrega:

```csharp
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));
```

Y define un contexto como este:

```csharp
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

    public DbSet<Producto> Productos => Set<Producto>();
}
```

Ejemplo de uso en una página o componente Blazor:

```razor
@page "/productos"
@inject AppDbContext DbContext

<h3>Productos</h3>

@code {
    private List<Producto> productos = new();

    protected override async Task OnInitializedAsync()
    {
        productos = await DbContext.Productos.ToListAsync();
    }
}
```

### 3. Publicar y copiar la aplicación
```powershell
dotnet publish -c Release -o publish
```
```bash
scp -r publish/ usuario@servidor:/var/www/miapp
```

### 4. Ejecutar con systemd
Crea un archivo de servicio en /etc/systemd/system/miapp.service con el siguiente contenido:

```ini
[Unit]
Description=Mi aplicación Blazor en Ubuntu
After=network.target

[Service]
WorkingDirectory=/var/www/miapp
ExecStart=/usr/bin/dotnet /var/www/miapp/MiAppBlazor.dll
Restart=always
RestartSec=10
User=www-data
Environment=ASPNETCORE_URLS=http://127.0.0.1:5000
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target
```

Activa y levanta el servicio:

```bash
sudo systemctl daemon-reload
sudo systemctl enable miapp
sudo systemctl start miapp
sudo systemctl status miapp
```

### 5. Configurar NGINX
Crea un bloque server en /etc/nginx/sites-available/miapp con este contenido:

```nginx
server {
    listen 80;
    server_name ejemplo.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Activa el sitio y reinicia NGINX:

```bash
sudo ln -s /etc/nginx/sites-available/miapp /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### 6. Validación
```bash
sudo systemctl status miapp
sudo journalctl -u miapp -f
```
