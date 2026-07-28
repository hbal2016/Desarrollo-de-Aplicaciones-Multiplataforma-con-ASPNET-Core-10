# Guía de despliegue en Fedora o Red Hat

## Objetivo
Desplegar una aplicación ASP.NET Core 10 en una distribución Fedora o Red Hat con PostgreSQL y NGINX.

## Pasos rápidos

### 1. Preparar el servidor
```bash
sudo dnf update -y
sudo dnf install -y dotnet-runtime-10.0 postgresql-server postgresql nginx
```

### 2. Configurar PostgreSQL
```bash
sudo postgresql-setup --initdb
```
Luego crea el usuario y la base de datos con psql.

### 3. Publicar y copiar la aplicación
Publica la aplicación desde tu equipo de desarrollo y copia los archivos al servidor.

### 4. Ejecutar con systemd
Configura un servicio systemd para ejecutar la aplicación en http://127.0.0.1:5000.

### 5. Configurar NGINX
Crea un bloque server en /etc/nginx/conf.d/ con proxy_pass al puerto local de la aplicación y reinicia NGINX.
