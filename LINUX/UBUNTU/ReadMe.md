# Guía de despliegue en Ubuntu

## 1. Preparar el servidor
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

## 2. Configurar PostgreSQL
1. Cambia al usuario postgres:
   ```bash
   sudo -u postgres psql
   ```
2. Crea usuario y base de datos:
   ```sql
   CREATE ROLE appuser WITH LOGIN PASSWORD 'TuPasswordSegura';
   CREATE DATABASE appdb OWNER appuser;
   \q
   ```

## 3. Publicar la aplicación
1. En tu máquina de desarrollo, publica la app:
   ```powershell
   dotnet publish -c Release -o publish
   ```
2. Copia la carpeta al servidor:
   ```bash
   scp -r publish/ usuario@servidor:/var/www/miapp
   ```

## 4. Ejecutar con systemd
1. Crea un archivo de servicio:
   ```bash
   sudo nano /etc/systemd/system/miapp.service
   ```
2. Añade:
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
3. Activa el servicio:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable miapp
   sudo systemctl start miapp
   ```

## 5. Configurar NGINX
1. Crea un host virtual:
   ```bash
   sudo nano /etc/nginx/sites-available/miapp
   ```
2. Agrega una configuración con proxy_pass a http://127.0.0.1:5000.
3. Habilita el sitio y reinicia NGINX:
   ```bash
   sudo ln -s /etc/nginx/sites-available/miapp /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx
   ```
