# Guía de desarrollo con VS Code

## Objetivo
Configurar un entorno ligero y práctico para desarrollar aplicaciones ASP.NET Core 10 y Blazor desde Visual Studio Code.

## 1. Descarga e instalación de VS Code

### Enlaces oficiales
- Descarga principal de VS Code: https://code.visualstudio.com/download
- Guía general de instalación: https://code.visualstudio.com/docs/setup/setup-overview
- Comparativa entre User Setup y System Setup (Machine Setup): https://code.visualstudio.com/docs/setup/windows#_user-setup-versus-system-setup

### Recomendación de instalación
- User Setup: ideal si vas a trabajar en tu propio equipo y quieres instalar VS Code solo para tu usuario.
- System Setup o Machine Setup: útil en equipos compartidos, laboratorios o entornos corporativos donde se desea instalar para todos los usuarios del equipo.

## 2. Herramientas recomendadas

Instala estas extensiones desde la pestaña de extensiones de VS Code:
- C# Dev Kit
- .NET Install Tool
- GitLens

Además, instala el SDK de .NET 10 y verifica la versión:

```bash
dotnet --version
```

## 3. Uso práctico de GitLens

GitLens amplía la experiencia de Git dentro de VS Code y ayuda a ver el historial de cambios de forma visual.

### Ejemplos de uso
- Ver el historial de un archivo:
  - Abre el archivo y usa la opción "GitLens: Open File History" desde la paleta de comandos.
- Revisar quién modificó una línea concreta:
  - Haz clic sobre la línea y selecciona "GitLens: Toggle Line Blame".
- Comparar cambios entre revisiones:
  - Selecciona un archivo y usa "GitLens: Compare with Previous Revision".
- Explorar commits recientes:
  - Abre la vista de Git y usa "GitLens: View Commit Details".

### Ejemplo rápido
1. Abre la paleta de comandos con Ctrl+Shift+P.
2. Escribe "GitLens".
3. Elige una acción como "Open File History" o "Toggle Line Blame".

## 4. Crear un proyecto Blazor o ASP.NET Core 10

### Crear un proyecto Blazor
```bash
dotnet new blazor -n MiAppBlazor
```

### Crear un proyecto ASP.NET Core web
```bash
dotnet new web -n MiAppAspNet
```

Abre la carpeta del proyecto desde VS Code y ejecuta:

```bash
dotnet run
```

## 5. Referencia rápida de documentación

Para consultar ejemplos y especificaciones rápidas de C# y ASP.NET Core 10, revisa la carpeta de documentación rápida:
- [Documentación rápida](../DOC/ReadMe.md)

## 6. Recursos oficiales de Microsoft
- C#: https://learn.microsoft.com/dotnet/csharp/
- ASP.NET Core: https://learn.microsoft.com/aspnet/core/
- Blazor: https://learn.microsoft.com/aspnet/core/blazor/
- Entity Framework Core: https://learn.microsoft.com/ef/core/
