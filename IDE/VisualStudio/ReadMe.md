# Guía de desarrollo con Visual Studio

## Objetivo
Preparar el entorno local en Windows para desarrollar una aplicación Blazor con ASP.NET Core 10.

## Pasos rápidos

### 1. Instalar herramientas
1. Descarga Visual Studio 2022 Community.
2. Activa las cargas de trabajo:
   - ASP.NET y desarrollo web
   - .NET desktop development
3. Instala el SDK de .NET 10.
4. Verifica la instalación:
   ```powershell
   dotnet --version
   ```

### 2. Crear el proyecto
1. Abre Visual Studio.
2. Selecciona Crear un proyecto.
3. Elige Blazor App o ASP.NET Core Web App.
4. Define el nombre y la ubicación del proyecto.
5. Haz clic en Crear.

### 3. Ejecutar la aplicación
1. Presiona F5.
2. Abre la URL local que aparece en el navegador.

### 4. Recomendaciones
- Usa Git para versionar el proyecto.
- Mantén separadas las configuraciones de desarrollo y producción.
- Usa appsettings.Development.json y appsettings.Production.json.
