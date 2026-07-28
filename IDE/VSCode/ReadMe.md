# Guía de desarrollo con VS Code

## 1. Instalar VS Code
1. Descarga Visual Studio Code desde la página oficial.
2. Instálalo en tu equipo.

## 2. Instalar extensiones recomendadas
Instala estas extensiones:
- C# Dev Kit
- .NET Install Tool
- GitLens
- REST Client

## 3. Instalar .NET 10 SDK
1. Descarga e instala el SDK de .NET 10.
2. Verifica la instalación:
   ```bash
   dotnet --version
   ```

## 4. Crear un proyecto Blazor
1. Abre una terminal en VS Code.
2. Crea una carpeta para el proyecto.
3. Ejecuta:
   ```bash
   dotnet new blazor -n MiAppBlazor
   ```
4. Abre la carpeta del proyecto en VS Code.

## 5. Ejecutar la aplicación
1. En la terminal, ejecuta:
   ```bash
   dotnet run
   ```
2. Abre la URL local en el navegador.
