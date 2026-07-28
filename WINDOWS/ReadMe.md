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

### 5. Configurar NGINX
Edita nginx.conf y añade un bloque server con proxy_pass a http://127.0.0.1:5000.
