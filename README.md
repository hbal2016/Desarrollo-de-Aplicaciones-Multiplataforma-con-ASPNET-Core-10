# Desarrollo de Aplicaciones Multiplataforma con ASP.NET Core 10 (Blazor)

Este manual documenta, paso a paso, cómo preparar un entorno de desarrollo para una aplicación web con ASP.NET Core 10 y Blazor, así como su despliegue en un servidor Linux Ubuntu y en un servidor Windows Server 2025, usando PostgreSQL como motor de bases de datos y NGINX como motor web. No se usa IIS en ninguna de las dos opciones de despliegue.

## 1. Entorno de desarrollo en Windows

### 1.1 Requisitos previos
- Windows 10/11 o Windows Server 2025 para desarrollo.
- Acceso a Internet para descargar herramientas.
- Permisos de administrador para instalar software.

### 1.2 Instalar herramientas base
1. Descarga e instala Visual Studio 2022 Community o Visual Studio Build Tools.
2. En el instalador, activa las cargas de trabajo:
   - ASP.NET y desarrollo web
   - .NET desktop development
3. Instala Git para Windows.
4. Instala el SDK de .NET 10 desde el sitio oficial de Microsoft.
5. Verifica la instalación:
   ```powershell
   dotnet --version
   git --version
   ```

### 1.3 Crear un proyecto Blazor
1. Abre PowerShell o Terminal.
2. Crea una nueva carpeta para el proyecto:
   ```powershell
   mkdir MiAppBlazor
   cd MiAppBlazor
   ```
3. Genera un proyecto Blazor con el siguiente comando:
   ```powershell
   dotnet new blazor -n MiAppBlazor
   ```
4. Entra a la carpeta del proyecto:
   ```powershell
   cd MiAppBlazor
   ```
5. Ejecuta la aplicación localmente:
   ```powershell
   dotnet run
   ```
6. Abre el navegador en la URL mostrada, normalmente:
   ```text
   http://localhost:5000
   ```

### 1.4 Configurar la base de datos con PostgreSQL
1. Instala PostgreSQL para Windows.
2. Crea una base de datos y un usuario de aplicación.
3. En el proyecto, agrega los paquetes NuGet necesarios:
   ```powershell
   dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
   dotnet add package Microsoft.EntityFrameworkCore.Design
   ```
4. Configura la cadena de conexión en appsettings.json:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Host=localhost;Database=miappdb;Username=postgres;Password=TuPassword"
     }
   }
   ```
5. Crea el contexto de la base de datos y aplica migraciones:
   ```powershell
   dotnet ef migrations add InitialCreate
   dotnet ef database update
   ```

### 1.5 Recomendaciones de desarrollo
- Usa Git para controlar versiones.
- Mantén la configuración de desarrollo separada de la de producción.
- Usa appsettings.Development.json y appsettings.Production.json.
- Para entornos de pruebas, considera usar Docker Desktop o una máquina virtual.

---

## 2. Despliegue en un servidor Linux Ubuntu con PostgreSQL y NGINX

### 2.1 Preparar el servidor Ubuntu
1. Actualiza el sistema:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Instala .NET 10 Runtime:
   ```bash
   sudo apt install -y dotnet-runtime-10.0
   ```
3. Instala PostgreSQL y NGINX:
   ```bash
   sudo apt install -y postgresql postgresql-contrib nginx ufw
   ```
4. Habilita el firewall:
   ```bash
   sudo ufw allow OpenSSH
   sudo ufw allow 'Nginx Full'
   sudo ufw enable
   ```

### 2.2 Configurar PostgreSQL
1. Cambia al usuario postgres:
   ```bash
   sudo -u postgres psql
   ```
2. Crea un usuario y una base de datos:
   ```sql
   CREATE ROLE appuser WITH LOGIN PASSWORD 'TuPasswordSegura';
   CREATE DATABASE appdb OWNER appuser;
   \q
   ```
3. Ajusta la autenticación según sea necesario en PostgreSQL.

### 2.3 Publicar la aplicación ASP.NET Core 10
1. En tu máquina de desarrollo, publica la aplicación:
   ```powershell
   dotnet publish -c Release -o publish
   ```
2. Copia la carpeta publicada al servidor Linux. Ejemplo:
   ```bash
   scp -r publish/ usuario@servidor:/var/www/miapp
   ```
3. Asegúrate de que la carpeta pertenezca al usuario adecuado:
   ```bash
   sudo chown -R www-data:www-data /var/www/miapp
   ```

### 2.4 Configurar la aplicación en producción
1. Crea o modifica el archivo de configuración de producción en el servidor:
   ```bash
   sudo nano /var/www/miapp/appsettings.Production.json
   ```
2. Agrega la cadena de conexión a PostgreSQL:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Host=127.0.0.1;Database=appdb;Username=appuser;Password=TuPasswordSegura"
     }
   }
   ```
3. Asegúrate de que la aplicación pueda escuchar en un puerto local como 5000 o 8080.

### 2.5 Crear un servicio systemd
1. Crea el archivo del servicio:
   ```bash
   sudo nano /etc/systemd/system/miapp.service
   ```
2. Agrega el contenido siguiente:
   ```ini
   [Unit]
   Description=Mi aplicación Blazor
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
3. Activa y levanta el servicio:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable miapp
   sudo systemctl start miapp
   sudo systemctl status miapp
   ```

### 2.6 Configurar NGINX como proxy inverso
1. Crea un host virtual:
   ```bash
   sudo nano /etc/nginx/sites-available/miapp
   ```
2. Agrega la siguiente configuración:
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
3. Activa el sitio:
   ```bash
   sudo ln -s /etc/nginx/sites-available/miapp /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx
   ```

### 2.7 Habilitar HTTPS con Let’s Encrypt (opcional, pero recomendado)
1. Instala Certbot:
   ```bash
   sudo apt install -y certbot python3-certbot-nginx
   ```
2. Genera el certificado:
   ```bash
   sudo certbot --nginx -d ejemplo.com
   ```

### 2.8 Validación final
- Abre la URL del dominio en el navegador.
- Revisa los logs si hay errores:
  ```bash
  sudo journalctl -u miapp -f
  ```

---

## 3. Despliegue en un servidor Windows Server 2025 con PostgreSQL y NGINX

### 3.1 Preparar el servidor Windows Server 2025
1. Instala el .NET 10 Runtime en el servidor.
2. Instala PostgreSQL para Windows.
3. Descarga NGINX para Windows y descomprímelo en una carpeta como:
   ```text
   C:\nginx
   ```
4. Abre el firewall para permitir los puertos 80 y 443.

### 3.2 Configurar PostgreSQL en Windows
1. Inicia PostgreSQL y crea una base de datos y un usuario:
   ```sql
   CREATE ROLE appuser WITH LOGIN PASSWORD 'TuPasswordSegura';
   CREATE DATABASE appdb OWNER appuser;
   ```
2. Verifica que el servidor acepte conexiones locales.

### 3.3 Publicar la aplicación ASP.NET Core 10
1. En tu máquina de desarrollo, publica la aplicación:
   ```powershell
   dotnet publish -c Release -o publish
   ```
2. Copia la carpeta publicada al servidor Windows, por ejemplo:
   ```text
   C:\inetpub\miapp
   ```

### 3.4 Configurar la aplicación en producción
1. Crea un archivo appsettings.Production.json en el servidor con la cadena de conexión:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Host=127.0.0.1;Database=appdb;Username=appuser;Password=TuPasswordSegura"
     }
   }
   ```
2. Ajusta la variable de entorno para que la aplicación se ejecute en un puerto local como 5000.

### 3.5 Ejecutar la aplicación como servicio Windows
1. Usa NSSM (Non-Sucking Service Manager) o el servicio de Windows para ejecutar la aplicación.
2. Ejemplo con NSSM:
   ```powershell
   nssm install MiAppApp "C:\Program Files\dotnet\dotnet.exe" "C:\inetpub\miapp\MiAppBlazor.dll"
   nssm set MiAppApp AppDirectory "C:\inetpub\miapp"
   nssm set MiAppApp AppEnvironmentExtra ASPNETCORE_URLS=http://127.0.0.1:5000
   nssm set MiAppApp AppEnvironmentExtra ASPNETCORE_ENVIRONMENT=Production
   nssm start MiAppApp
   ```
3. Verifica que el servicio está activo.

### 3.6 Configurar NGINX para Windows
1. Edita el archivo de configuración de NGINX:
   ```text
   C:\nginx\conf\nginx.conf
   ```
2. Agrega una configuración similar a esta:
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
3. Inicia NGINX desde la consola o como servicio.

### 3.7 Validación final
- Abre el dominio o la IP del servidor en el navegador.
- Revisa los logs del servicio si la aplicación no responde.

---

## 4. Recomendaciones de producción
- Usa variables de entorno en lugar de escribir secretos directamente en los archivos de configuración.
- Mantén copias de seguridad de la base de datos.
- Considera activar HTTPS con certificados válidos en ambos servidores.
- Usa supervisión y registros para detectar fallos de forma temprana.

## 5. Resumen ejecutivo
- El entorno de desarrollo se puede preparar fácilmente en Windows con Visual Studio y .NET 10.
- La aplicación se puede publicar y ejecutar en Linux Ubuntu con PostgreSQL y NGINX como proxy inverso.
- La misma aplicación puede desplegarse en Windows Server 2025 usando PostgreSQL para Windows y NGINX para Windows, sin depender de IIS.
