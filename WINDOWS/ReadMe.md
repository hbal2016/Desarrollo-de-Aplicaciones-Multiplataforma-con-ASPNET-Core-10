# Guía de despliegue en Windows

## Objetivo
Desplegar una aplicación ASP.NET Core 10 en un servidor Windows con PostgreSQL y NGINX, sin usar IIS.

## Pasos rápidos

### 1. Preparar el servidor
1. Instala el runtime de .NET 10.
2. Instala PostgreSQL para Windows.
3. Descarga NGINX para Windows y descomprímelo en C:\nginx.

### 2. Configurar PostgreSQL
Crea una base de datos y un usuario con permisos, y actualiza la cadena de conexión en la aplicación.

### 3. Publicar la aplicación
```powershell
dotnet publish -c Release -o publish
```
Copia la carpeta publicada al servidor Windows.

### 4. Ejecutar la aplicación
Usa NSSM o un servicio de Windows para ejecutar el archivo DLL y configura la variable de entorno:
```powershell
ASPNETCORE_URLS=http://127.0.0.1:5000
```

Ejemplo con NSSM:

```powershell
nssm install MiAppApp "C:\Program Files\dotnet\dotnet.exe"
"C:\inetpub\miapp\MiAppBlazor.dll"
nssm set MiAppApp AppDirectory "C:\inetpub\miapp"
nssm set MiAppApp AppEnvironmentExtra "ASPNETCORE_URLS=http://127.0.0.1:5000"
nssm set MiAppApp AppEnvironmentExtra "ASPNETCORE_ENVIRONMENT=Production"
nssm start MiAppApp
```

### 5. Configurar NGINX
Edita el archivo de configuración de NGINX, por ejemplo C:\nginx\conf\nginx.conf, con este contenido:

```nginx
worker_processes 1;

events {
    worker_connections 1024;
}

http {
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
}
```

Inicia NGINX desde la consola o como servicio:

```powershell
cd C:\nginx
nginx.exe
```