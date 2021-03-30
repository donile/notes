# Filters

Components that allow logic to be injected at various stages of the MVC life cycle.

Filters are executed by the `ControllerActionInvoker` class.

Authorization, Resource and Middleware Filters are executed before the Controller is instantiated.

Action Filters are executed before and after the Action Method is executed.

Result Filters are executed before and after the Result is executed.

Exception Filters may be executed at any time throughout the MVC life cycle.

## Filter Scopes

Controller Level  
Action Method Level  

## Authorization Filters

Inherit from the `Attribute` class.

Implement the `IAuthorizationFilter` interface

Implement custom security logic within the `OnAuthorization()` method of the `IAuthorizationFilter` interface.

Can short circuit the MVC life cycle by providing a `Result` object.

### Implement a Custom Authorization Filter

```csharp
public class MyAuthorizationFilter : Attribute, IAuthorizationFilter
{

    private readonly MyDependency _myDependency;

    public MyAuthorizationFilter(MyDependency myDependency)
    {
        _myDependency = myDependency;
    }

    public void OnAuthorization(AuthorizationFilterContext context)
    {
        if(myDependency.isNotAuthorized())
        {
            // set the Result to short circuit the MVC life cycle
            // and prevent further evaluation of the request
            context.Result = new UnauthorizedResult();
        }
    }
}
```

### Apply a Custom Authorization Filter

```csharp
// The TypeFilter attribute is necessary to inject MyAuthorizationFilater's dependency
[TypeFilter(typeof(MyAuthorizationFilter))]
public class MyController
{
    // Controller implementation...
}
```

## Resource Filters

Implement `IResourceFilter` interface

Expose `OnResourceExecuting()` and `OnResourceExecuted()` methods

Wrap ALMOST the entire MVC life cycle process

Execute after Authorization Filters and before the MVC life cycle ends

### Use Cases

Caching  
Short-circuiting  
Inject logic very early or late in MVC life cycle  

## Middleware Filters

Execute standard Middleware components within the MVC life cycle

### Use Cases

Reuse or execute Middleware components later in the request life cycle

Provide more information to the Middleware component that is available within the MVC life cycle

### Middleware Filter Implementation

```csharp
[MiddlewareFilter(typeof(MiddlewareWrapper))]
public class MyController
{
    // MyController implementation...
}
```

```csharp
public class MiddlewareWrapper
{
    public void Configure(IApplicationBuilder app)
    {
        app.UseMiddleware<Middleware>();
    }
}
```