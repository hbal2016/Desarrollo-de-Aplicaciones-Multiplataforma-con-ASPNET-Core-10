# Guía de desarrollo con VS Code

## Objetivo
Configurar un entorno ligero y práctico para desarrollar aplicaciones Blazor desde Visual Studio Code.

## Pasos rápidos

### 1. Instalar herramientas
1. Descarga e instala VS Code.
2. Instala estas extensiones:
   - C# Dev Kit
   - .NET Install Tool
   - GitLens
3. Instala el SDK de .NET 10.
4. Verifica la instalación:
   ```bash
   dotnet --version
   ```

### 2. Crear el proyecto
1. Abre una terminal en VS Code.
2. Crea una carpeta para el proyecto.
3. Ejecuta:
   ```bash
   dotnet new blazor -n MiAppBlazor
   ```
4. Abre esa carpeta en VS Code.

### 3. Ejecutar la aplicación
1. En la terminal, ejecuta:
   ```bash
   dotnet run
   ```
2. Abre la URL local en el navegador.
