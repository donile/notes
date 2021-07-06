# Adding an Action to an Entity Set

## Add the Action to the Entity Data Model

```csharp
public class MyEntityDataModel
{
    public IEdmModel GetEntityDataModel()
    {
        var builder = new ODataConventionModelBuilder();
        builder.Namespace = "MyNameSpace";
        builder.ContainerName = "MyContainer";
        
        builder.EntitySet<MyClass>("MyClass");

        var customAction 
            = builder
                .EntitySet<MyClass>("MyClass")
                .Collection()
                .Action("ActionName");
        
        customAction
            .Parameter<int>("paramName1");
        
        customAction
            .Parameter<int>("paramName2");
        
        customAction
            .ReturnsCollectionForEntitySet<MyClass>("MyClass");

        customAction
            .NameSpace = "MyNameSpace";
```

## Implementing an Action OData API Endpoint

Actions are executed via the HTTP POST method (per the OData standard).

HTTP GET requests should not produce side-effects.  POST requests may produce side-effects, just like Actions, thus the HTTP POST method is used for Actions.

The HTTP request body will contain the parameter values.

```csharp
[HttpPost("Items/MyNameSpace.Actions.ActionName")]
public async Task<IActionResult> ActionName(ODataActionParameters parameters)
{
    ...
}
```

## Calling an OData Action

```http
POST http://my.domain.com/odata/Items/MyNameSpace.Actions.ActionName
Content-Type: application/json

{
    "paramName1": "paramValue1"
    "paramName2": "paramValue2"
}
```
