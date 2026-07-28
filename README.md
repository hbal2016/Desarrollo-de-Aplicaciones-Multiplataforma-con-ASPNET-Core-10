# Desarrollo de Aplicaciones Multiplataforma con ASP.NET Core 10 (Blazor)

Esta guía reúne, de forma organizada, los pasos para desarrollar y desplegar una aplicación web con ASP.NET Core 10 y Blazor en distintos entornos.

## Cómo usar esta guía

1. Elige tu entorno de desarrollo:
   - [Visual Studio](IDE/VisualStudio/ReadMe.md)
   - [VS Code](IDE/VSCode/ReadMe.md)
2. Elige tu servidor de despliegue:
   - [Ubuntu](LINUX/UBUNTU/ReadMe.md)
   - [Fedora / Red Hat](LINUX/FEDORA/ReadMe.md)
   - [Windows](WINDOWS/ReadMe.md)
3. Usa PostgreSQL como base de datos y NGINX como proxy inverso.

## Resumen general

- Desarrollo local: Windows + Visual Studio o VS Code + .NET 10 + Blazor.
- Producción: aplicación publicada + PostgreSQL + NGINX.
- No se usa IIS en ninguna opción de despliegue.

## Objetivo del repositorio

Organizar la documentación por categorías para que resulte más fácil encontrar la información según el escenario:

- IDE para desarrollo local
- Linux para despliegue en distribuciones Ubuntu y Fedora/Red Hat
- Windows para despliegue en servidores Windows
