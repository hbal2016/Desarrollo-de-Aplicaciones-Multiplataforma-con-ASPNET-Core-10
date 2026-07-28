# Guía de despliegue en Windows

## 1. Preparar el servidor
1. Instala el runtime de .NET 10 en el servidor Windows.
2. Instala PostgreSQL para Windows.
3. Descarga NGINX para Windows y descomprímelo en una carpeta como:
   ```text
   C:\nginx
   ```

## 2. Configurar PostgreSQL
1. Crea una base de datos y un usuario con permisos.
2. Ajusta la cadena de conexión en la aplicación.

## 3. Publicar la aplicación
1. Publica la app desde el equipo de desarrollo:
   ```powershell
   dotnet publish -c Release -o publish
   ```
2. Copia la carpeta publicada al servidor Windows.

## 4. Ejecutar la aplicación
1. Usa NSSM o un servicio de Windows para ejecutar el archivo DLL de la aplicación.
2. Configura la variable de entorno:
   ```powershell
   ASPNETCORE_URLS=http://127.0.0.1:5000
   ```

## 5. Configurar NGINX
1. Edita el archivo nginx.conf.
2. Añade un bloque server con proxy_pass a http://127.0.0.1:5000.
3. Inicia NGINX desde la consola o como servicio.
