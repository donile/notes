# Defining an Entity Model

## Install NuGet Packages
`Microsoft.AspNetCore.OData 8.0.0-rc2`

## Defining the Entity Model

```csharp
using Microsoft.OData.Edm;
using Microsoft.OData.ModelBuilder;

public class MyEntityDataModel
{
    public IEdmModel GetEntityDataModel()
    {
        var builder = new ODataConventionModelBuilder();
        builder.Namespace = "MyNameSpace";
        builder.ContainerName = "MyContainer";

        builder.EntitySet<MyClass1>("MyClass1");
        builder.EntitySet<MyClass2>("MyClass2");
        return builder.GetEdmModel();
    }
}
```

## Add the OData Service to the IOC Container

Calling the `IServiceCollection.AddOData()` extension method:
- Adds the endpoint that serves the `service document` from the root endpoint  The root endpoint for the example configured below is `/odata`.
- Adds the endpoint that serves the metadata document from the `/root/$metadata` endpoint.
- Enables convention based routing (use attribute based routing per Microsoft's recommendation)

```csharp
using Microsoft.AspNetCore.OData;
...

public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddOData(options => {
        options.AddModel("odata", new MyEntityDataModel().GetEntityDataModel());
    });
}
```

## GET service document
`http://hostname:port/odata/`

## GET metadata document
`http://hostname:port/odata/$metadata`
