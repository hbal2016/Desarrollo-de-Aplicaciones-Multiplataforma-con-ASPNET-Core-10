# Quick Ref: Programación orientada a objetos en C#

## Clase y objeto
```csharp
public class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }

    public void Saludar()
    {
        Console.WriteLine($"Hola, soy {Nombre}");
    }
}
```

## Herencia
```csharp
public class Estudiante : Persona
{
    public string Carrera { get; set; }
}
```

## Encapsulamiento
```csharp
public class Cuenta
{
    private decimal saldo;

    public decimal Saldo
    {
        get { return saldo; }
        set { saldo = value; }
    }
}
```

## Enlaces oficiales
- Clases y objetos: https://learn.microsoft.com/dotnet/csharp/fundamentals/tutorials/classes
- Herencia y polimorfismo: https://learn.microsoft.com/dotnet/csharp/fundamentals/tutorials/inheritance
