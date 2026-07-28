# Guía de despliegue en Fedora o Red Hat

## Objetivo
Desplegar una aplicación ASP.NET Core 10 en una distribución Fedora o Red Hat con PostgreSQL y NGINX.

## Pasos rápidos

### 1. Preparar el servidor
Actualiza el sistema e instala los componentes base:

```bash
sudo dnf update -y
sudo dnf install -y curl gpg ca-certificates
```

Instala el runtime de .NET 10 usando los repositorios de la propia distribución Fedora:

```bash
sudo dnf install -y dotnet-runtime-10.0
```

Si no aparece el paquete en tu versión, puedes verificarlo con:

```bash
sudo dnf search dotnet-runtime-10.0
```

Instala PostgreSQL y NGINX desde los repositorios de la propia distribución:

```bash
sudo dnf install -y postgresql-server postgresql nginx
```

Si necesitas verificar el paquete disponible en tu versión concreta:

```bash
sudo dnf search postgresql
sudo dnf search nginx
```

Enlaces oficiales de referencia:
- Fedora / Red Hat: https://learn.microsoft.com/dotnet/core/install/linux
- PostgreSQL: https://www.postgresql.org/download/linux/redhat/
- NGINX: https://nginx.org/en/linux_packages.html

### 2. Configurar PostgreSQL
```bash
sudo postgresql-setup --initdb
```
Luego crea el usuario y la base de datos con psql:

```bash
sudo -u postgres psql
```
```sql
CREATE ROLE appuser WITH LOGIN PASSWORD 'TuPasswordSegura';
CREATE DATABASE appdb OWNER appuser;
\q
```

Ejemplo de archivo de configuración en /var/www/miapp/appsettings.Production.json:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=127.0.0.1;Port=5432;Database=appdb;Username=appuser;Password=TuPasswordSegura"
  }
}
```

En Program.cs de la aplicación Blazor:

```csharp
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));
```

Y en un componente:

```razor
@inject AppDbContext DbContext

@code {
    private List<Producto> productos = new();

    protected override async Task OnInitializedAsync()
    {
        productos = await DbContext.Productos.ToListAsync();
    }
}
```

### 3. Publicar y copiar la aplicación
Publica la aplicación desde tu equipo de desarrollo y copia los archivos al servidor.

### 4. Ejecutar con systemd
Crea un archivo de servicio en /etc/systemd/system/miapp.service con contenido similar a este:

```ini
[Unit]
Description=Mi aplicación Blazor en Fedora
After=network.target

[Service]
WorkingDirectory=/var/www/miapp
ExecStart=/usr/bin/dotnet /var/www/miapp/MiAppBlazor.dll
Restart=always
RestartSec=10
User=nginx
Environment=ASPNETCORE_URLS=http://127.0.0.1:5000
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target
```

Luego activa el servicio:

```bash
sudo systemctl daemon-reload
sudo systemctl enable miapp
sudo systemctl start miapp
sudo systemctl status miapp
```

### 5. Configurar NGINX
Crea un archivo en /etc/nginx/conf.d/miapp.conf con este contenido:

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

Reinicia NGINX:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl reload nginx
```
