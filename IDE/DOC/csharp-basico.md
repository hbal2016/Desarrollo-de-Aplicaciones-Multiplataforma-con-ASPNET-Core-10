# Quick Ref: C# básico

## Sintaxis general
```csharp
using System;

namespace MiApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hola, mundo");
        }
    }
}
```

## Variables y tipos
```csharp
int edad = 25;
double precio = 19.99;
string nombre = "Ana";
bool activo = true;
```

## Estructuras de control
```csharp
if (edad >= 18)
{
    Console.WriteLine("Mayor de edad");}

for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);
}
```

## Enlaces oficiales
- C# guía rápida: https://learn.microsoft.com/dotnet/csharp/tour-of-csharp/
- Sintaxis de C#: https://learn.microsoft.com/dotnet/csharp/language-reference/
