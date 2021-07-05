# Adding Custom Functions to an Entity Set

Add a custom function that executes against the context of the entity set, not just an individual entity.

```csharp
public class MyEntityDataModel
{
    public IEdmModel GetEntityDataModel()
    {
        var builder = new ODataConventionModelBuilder();
        builder.Namespace = "MyNameSpace";
        builder.ContainerName = "MyContainer";

        builder.EntitySet<MyClass>("MyClass");
                
        var customEntitySetFunction 
            = builder
                .EntitySet<MyClass>() 
                .Collection()
                .Function("FunctionName");
        
        customEntitySetFunction
            .CollectionParameter<string>("paramName");

        customEntitySetFunction
            .ReturnsCollectionFromEntitySet<MyClass>("MyClass");

        customEntitySetFunction
            .Namespace = "MyNameSpace.Functions";

        return builder.GetEdmModel();
    }
}
```

## Adding the Custom Entity Set Function OData API Endpoint

Use the `FromODataUri` attribute to bind a list of parameters in the OData URI to an `IEnumerable<T>` action method parameter (see example URI below).

```csharp
using Microsoft.AspNetCore.OData.Formatter;

[HttpGet("Items/MyNameSpace.Functions.FunctionName(paramName={paramValue})")]
public async Task<IActionResult> FunctionName([FromODataUri] IEnumberable<string> paramValue)
{
    ...
}
```

## Calling the Custom Entity Set Function OData API Endpoint

`http://my.domain.com/odata/Items/MyNameSpace.Functions.FunctionName(paramValue=[string1,string2])`
