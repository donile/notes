# Configuring Query Options

## Configure the `$select` query parameter

Call the `Select()`, `Expand()` methods.

```csharp
using Microsoft.AspNetCore.OData;
...

public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddOData(options => {
        options.AddModel("odata", new MyEntityDataModel().GetEntityDataModel))
        .Select()
        .Expand()
    });
}
```

## Enable `$select` Query Parameter on Action Method

Apply `EnableQuery` attribute to the action method.

Presently, the `EnableQuery` attribute cannot be applied to an `async` action method.

```csharp
using Microsoft.AspNetCore.OData.Query;
...

[EnableQuery]
public IActionResult GetItems()
{
    ...
}
```
