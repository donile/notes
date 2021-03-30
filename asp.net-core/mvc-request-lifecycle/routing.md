# Routing

The process of mapping an incoming request to an endpoint.

## Routing Types

### Conventional Routing
Templates that map an incoming request to a Controller and Action Method combination.

Registered in the `StartUp` class.

```csharp
public void Configure(IApplicationBuilder app)
{
    // ...
    app.UseEndpoints(endpoints => {
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Home}/{action=Index}/{id?}"
        );
    });
}
```

### Attribute Routing
Attributes applied to Controller class or Action method.

```csharp
[Route("parent/{parentId}")]
public class ChildController
{
    [Route("child/{id}")]
    public IActionResult GetChildForParent(int id)
    {
        // return the child object with id and parentId
    }
}
```

## Route Execution Order

1. Attribute routes are evaluated in parallel.
2. Conventional routes are evaluated in the order in which they are registered.

## Endpoint Routing System

Composed of two middleware components:
1. Endpoint Routing Middleware
2. Endpoint Middleare

### Endpoint Routing Middleware

Selects the endpoint that will handle the request.

### Endpoint Middleware

Executes the selected endpoint.

### Endpoints

Classes that contain a Request Delegate and other metadata used to generate the response.

Contain metadata to determine if it should be executed for the current request.

The `ControllerActionEndpointDataSource` class is created within `StartUp.ConfigureServices()`.  

When the first request is received by the application, within the `EndpointRoutingMiddleware` the `ControllerActionEndpointDataSource.CreateEndpoints()` method is called and retrieves all of the Controllers and Action Methods. For each Controller and Action Method, an Endpoint object is created and stored in the `EndpointDataSource`.  Route templates are also stored in the `EndpointDataSource`.  Next the `EndpointRoutingMiddleware` selects the `Endpoint` that will handle the request.

After selecting the `Endpoint` to handle the request, the `EndpointRoutingMiddleware` forwards the selected `Endpoint` along the middleware pipeline by adding it to the `HttpContext`.  Other middleware components may use the selected `Endpoint`. Finally the `EndpointMiddleware` will execute the selected `Endpoint` to generate the response.

### Request Delegate

Wrapper around a method that instantiates a Controller executes the Action Method.
