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
