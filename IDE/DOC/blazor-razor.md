# Quick Ref: Blazor y Razor

## Componente simple en Blazor
```razor
@page "/contador"

<h3>Contador</h3>
<p>Valor actual: @currentCount</p>
<button @onclick="Incrementar">Incrementar</button>

@code {
    private int currentCount = 0;

    private void Incrementar()
    {
        currentCount++;
    }
}
```

## Sintaxis principal
- @page: define la ruta del componente.
- @code: contiene lógica C# del componente.
- @onclick: enlaza eventos del navegador.

## Enlaces oficiales
- Blazor overview: https://learn.microsoft.com/aspnet/core/blazor/
- Componentes Razor: https://learn.microsoft.com/aspnet/core/blazor/components/
