# Custom Functions

## Adding a Custom Function

Add the custom function to the Entity Model.

```csharp
public class MyEntityDataModel
{
    public IEdmModel GetEntityDataModel()
    {
        var builder = new ODataConventionModelBuilder();
        builder.Namespace = "MyNameSpace";
        builder.ContainerName = "MyContainer";

        builder.EntitySet<MyClass>("MyClass");

        var customEntityFunction 
            = builder
                .EntitySet<MyClass3>("MyClass")
                .Function("FunctionName");
        
        customEntityFunction
            .Parameter<int>("paramName");
        
        customEntityFunction
            .Returns<bool>();

        return builder.GetEdmModel();
    }
}
```

## Adding the Custom Function OData API Endpoint

```csharp
[HttpGet("Items({id})/MyNameSpace.Functions.FunctionName(paramName={paramValue})")]
public async Task<bool> FunctionName(int paramValue)
{
    ...
}
```

## Calling the Custom Entity Function OData API Endpoint

`http://my.domain.com/odata/Items/MyNameSpace.Functions.FunctionName(paramValue=1)`
