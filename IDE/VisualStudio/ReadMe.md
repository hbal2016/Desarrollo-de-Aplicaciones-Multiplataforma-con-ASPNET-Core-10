# Guía de desarrollo con Visual Studio

## 1. Instalar Visual Studio 2022
1. Descarga Visual Studio 2022 Community desde el sitio oficial de Microsoft.
2. Ejecuta el instalador y selecciona las cargas de trabajo:
   - ASP.NET y desarrollo web
   - .NET desktop development
3. Completa la instalación y reinicia el equipo si es necesario.

## 2. Instalar .NET 10 SDK
1. Descarga el SDK de .NET 10 desde Microsoft.
2. Instálalo y confirma la versión:
   ```powershell
   dotnet --version
   ```

## 3. Crear un proyecto Blazor
1. Abre Visual Studio.
2. Selecciona Crear un proyecto.
3. Elige Blazor App o ASP.NET Core Web App.
4. Define el nombre del proyecto y la ruta.
5. Haz clic en Crear.

## 4. Ejecutar la aplicación
1. Presiona F5 o selecciona Iniciar depuración.
2. Abre la URL mostrada en el navegador.

## 5. Recomendaciones
- Usa Git para versionar el proyecto.
- Mantén separado el entorno de desarrollo y producción.
- Configura appsettings.Development.json y appsettings.Production.json.
