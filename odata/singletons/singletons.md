# Singletons

One entity within the model, thus no id is necessary.

No entity set is required either, because their is only one.

The singleton can be retrieved without addressing an entity set or providing an id.

Singletons should always exist and therefore there should **NOT** be endpoints to create or delete the singleton.

Singletons can be updated via an HTTP PATCH request.

## Adding a Singleton to the Entity Model

```csharp
public class MyEntityDataModel
{
    public IEdmModel GetEntityDataModel()
    {
        var builder = new ODataConventionModelBuilder();
        builder.Namespace = "MyNameSpace";
        builder.ContainerName = "MyContainer";
        
        builder.Singleton<MyClass>("MySingleton");

        return builder.GetEdmModel();
    }     
```

## Implementing a Singleton OData API Endpoint

```csharp
[HttpGet("MySingleton")]
public async Task<IActionResult> GetMySingleton()
{
    ...
}
```
