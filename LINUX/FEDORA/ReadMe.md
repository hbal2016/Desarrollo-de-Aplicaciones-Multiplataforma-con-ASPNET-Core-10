# Guía de despliegue en Fedora o Red Hat

## 1. Preparar el servidor
1. Actualiza el sistema:
   ```bash
   sudo dnf update -y
   ```
2. Instala .NET 10 Runtime:
   ```bash
   sudo dnf install -y dotnet-runtime-10.0
   ```
3. Instala PostgreSQL y NGINX:
   ```bash
   sudo dnf install -y postgresql-server postgresql nginx
   ```

## 2. Configurar PostgreSQL
1. Inicializa la base de datos si es necesario:
   ```bash
   sudo postgresql-setup --initdb
   ```
2. Crea usuario y base de datos con el cliente psql.

## 3. Publicar y ejecutar la aplicación
1. Publica la aplicación desde Windows o una máquina de desarrollo.
2. Copia los archivos al servidor.
3. Usa systemd para ejecutar el proceso de la aplicación igual que en Ubuntu.

## 4. Configurar NGINX
1. Crea un archivo de configuración en /etc/nginx/conf.d/.
2. Define un bloque server con proxy_pass a la URL local de la app.
3. Reinicia NGINX:
   ```bash
   sudo systemctl enable nginx
   sudo systemctl start nginx
   ```
