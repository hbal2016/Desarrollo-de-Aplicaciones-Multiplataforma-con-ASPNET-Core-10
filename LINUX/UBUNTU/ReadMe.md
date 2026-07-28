# Guía de despliegue en Ubuntu

## Objetivo
Desplegar una aplicación ASP.NET Core 10 en un servidor Ubuntu con PostgreSQL y NGINX.

## Pasos rápidos

### 1. Preparar el servidor
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y dotnet-runtime-10.0 postgresql postgresql-contrib nginx ufw
```

### 2. Configurar PostgreSQL
```bash
sudo -u postgres psql
```
```sql
CREATE ROLE appuser WITH LOGIN PASSWORD 'TuPasswordSegura';
CREATE DATABASE appdb OWNER appuser;
\q
```

### 3. Publicar y copiar la aplicación
```powershell
dotnet publish -c Release -o publish
```
```bash
scp -r publish/ usuario@servidor:/var/www/miapp
```

### 4. Ejecutar con systemd
Crea un archivo de servicio en /etc/systemd/system/miapp.service y configura el comando para ejecutar la aplicación en http://127.0.0.1:5000.

### 5. Configurar NGINX
Crea un bloque server en /etc/nginx/sites-available/miapp con proxy_pass a http://127.0.0.1:5000 y habilítalo.

### 6. Validación
```bash
sudo systemctl status miapp
sudo systemctl reload nginx
```
