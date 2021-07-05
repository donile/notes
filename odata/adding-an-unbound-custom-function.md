# Adding an Unbound Custom Function

The function is not bound to a an entity.

Instead, the function is bound to the root level.

## Add an Unbound Function to the Entity Model.

```csharp
public class MyEntityDataModel
{
    public IEdmModel GetEntityDataModel()
    {
        var builder = new ODataConventionModelBuilder();
        builder.Namespace = "MyNameSpace";
        builder.ContainerName = "MyContainer";

        var customEntitySetFunction 
            = builder
                .EntitySet<MyClass3>("MyClass");

        var customUnboundFunction 
            = builder.Function("FunctionName");
        
        customUnboundFunction
            .Parameter<int>("paramName");
        
        customUnboundFunction
            .ReturnsCollectionFromEntitySet<MyClass>("MyClass");

        customUnboundFunction
            .NameSpace = "MyNameSpace";
```

## Implement the Unbound Function OData API Endpoint

```csharp
[HttpGet("FunctionName(paramName={paramValue})")]
public async Task<IActionResult> FunctionName(int paramValue)
{
    ...
}
```

## Calling the Unbound Custom Function OData API Endpoint

`http://my.domain.com/odata/FunctionName(paramValue=1)`
