# Middleware

The series of components that form the application request pipeline.

The order in which middleware components are registered is important because each middleware component is executed in the order in which it is registered.

Each middleware component can act on the incoming request and outgoing response.

Middleware may short-ciruit a response and prevent middleware components after it from executing on the request.

### Examples

Routing  
Session  
CORS  
Authentication  
Caching  

## Creating Middleware

Middleware may:

Generate a response (prevent additional middleware in the pipeline from executing), perform work using the request, or reroute the request to a different middleware path.

### Generating a Response
Use the `IApplicationBuilder.Run()` method to generate a response.

```csharp
app.Run(async httpContext => await httpContext.Response.WriteAsync("Hello, World!));
```

### Performing Work with Request
Use the `IApplicationBuilder.Use()` method to perform work and pass along the HTTP request.

```csharp
app.Use(async (httpContext, next) =>
    {
        // do stuff before passing along HTTP request...

        await next.Invoke();

        // do stuff after downstream middleware has executed...
    }
);
```

### Reroute Request to a Different Middleware Path
Use the `IApplicationBuilder.Map()` method to route the request to a handler method.

```csharp
app.Map("/hello-world", SayHello);

private static void SayHello(IApplicationBuilder app)
{
    app.Run(async httpContext => {

        await httpContext.Response.WriteAsync("Hello, World!");

    });
}
```

## Custom Middleware Classes
Middleware classes do not have to inherit from a base class or implement an interface.  Services may be injected into Middleware by adding the service as a parameter to the `Invoke()` method.

```csharp
public class CustomMiddleware
{
    private RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public Task Invoke(HttpContext httpContext)
    {
         // do stuff before passing along HTTP request...

        await next.Invoke(httpContext);

        // do stuff after downstream middleware has executed...
    }
}
```

## Add Middleware Class to Pipeline

```csharp
public void Configure(IApplicationBuilder app)
{
    // ...

    app.UseMiddleware<CustomMiddleware>();

    // ...
}
```