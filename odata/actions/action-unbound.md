# Adding an Action to an Entity

## Add the Action to the Entity Data Model

```csharp
public class MyEntityDataModel
{
    public IEdmModel GetEntityDataModel()
    {
        var builder = new ODataConventionModelBuilder();
        builder.Namespace = "MyNameSpace";
        builder.ContainerName = "MyContainer";
        
        var customAction 
            = builder
                .Action("ActionName");
        
        customAction
            .Parameter<int>("paramName");
        
        customAction
            .Returns<bool>();

        customAction
            .NameSpace = "MyNameSpace";
```

## Implementing an Action OData API Endpoint

Actions are executed via the HTTP POST method (per the OData standard).

HTTP GET requests should not produce side-effects.  POST requests may produce side-effects, just like Actions, thus the HTTP POST method is used for Actions.

The HTTP request body will contain the parameter values.

```csharp
[HttpPost("ActionName")]
public async Task<IActionResult> ActionName(ODataActionParameters parameters)
{
    ...
}
```

## Calling an OData Action

```http
POST http://my.domain.com/odata/ActionName
Content-Type: application/json

{
    "paramName": "paramValue"
}
```
