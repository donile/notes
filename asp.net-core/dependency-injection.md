# Dependency Injection

## Service Descriptors

Adding a service to the `IServiceCollection` inserts a `ServiceDescriptor`.  `ServiceDescriptor` instances contain information about the registered service.  For example, the `ServiceDescriptor` class has `ServiceType`, `ImplementationType` and `ServiceLifetime` properties.

## Adding Services to the `IServiceCollection`

There are several methods that allow a service to be added to the `IServiceCollection` method.

#### `IServiceCollection` `Add()` Method Examples

```csharp
AddTransient<TService, TImplementation>();
AddScoped<TService, TImplementation>();
AddSingleton<TService, TImplementation>();
```

Using the `Add()` method will insert the service into the `IServiceCollection`.  Future calls to an `Add()` method override previous calls, meaning only the service that was added to the `IServiceCollection` last can be retrieved from the container.

Additional calls to `Add()` methods do **NOT** replace previously registered services.  Instead, multiple implementations are registered for the service type.  The container returns the last registered implementation.

### `IServiceCollection` `TryAdd()` Method Examples

The `TryAdd()` methods do **NOT** override services previously registered with the container.  Instead, if an implementation has already been registered for the service type, the previously registered implementation will continue to be returned by the container when the service is requested.

The `TryAdd()` methods do not throw exceptions.

## Replacing Services in `IServiceCollection`

```csharp
IServiceCollection.Replace(ServiceDescriptor serviceDescriptor)
```

The `Replace()` method only replaces the most recently registered service implementation, which makes the newly inserted service implementation the object returned by the container.

## Removing all Service Implementations from `IServiceCollection`

```csharp
IServiceCollection.RemoveAll<TService>()
```

## Adding Multiple Service Implementations

If multiple service implementations are added to the `IServiceCollection` and a class' constructor contains an argument of type `IEnumerable<TServiceInterface>`, the `IServiceProvider` will return an `IEnumerable<TServiceInterface>` that contains all of the services in the `IServiceCollection` that are registered as implementations of the `TServiceInterface`.

## Avoid Adding Duplicate Service Implementations

Use the `IServiceCollection.TryAddEnumerable(ServiceDescriptor descriptor)` method to add multiple service implementations for the service interface **WITHOUT** adding a duplicate service implementation.

For example, the code below will only add the `TServiceImplementation1` to the container once even though the `TryAddEnumerable()` method was called twice for `TServiceImplementation1`.

```csharp
services.TryAddEnumerable(ServiceDescriptor.Transient<TServiceInterface, TServiceImplementation1>);
services.TryAddEnumerable(ServiceDescriptor.Transient<TServiceInterface, TServiceImplementation2>);services.TryAddEnumerable(ServiceDescriptor.Transient<TServiceInterface, TServiceImplementation1>);
```

## Adding a Service Implementation Factory

When a service instance must be built using a method other than a constructor, for example, when third-party code uses a factory or builder pattern, use a service implementation factory method to inform the `IServiceCollection` about how the service instance should be constructed.

Below is an example of how to add a service implementation factory to the `IServiceCollection`.

```csharp
services.AddTransient<IServiceInterface>(sp => 
    {
        // use services added to IServiceCollection
        // and build and return new IServiceImplementation
    });
```

## Adding a Service That Implements Multiple Interfaces

## Adding a Generic Service Implementation

```csharp
services.AddTransient(typeof(IServiceInterface<>), typeof(ServiceImplementation));
```

## Custom `IServiceCollection` `Add()` Extension Methods

Prefer adding the extension methods to the `Microsoft.Extensions.DependencyInjection` namespace.  Since the `StartUp` class imports the `Microsoft.Extensions.DependencyInjection` namespace, this will automatically include the extension methods in the `StartUp` class.

```csharp
namespace Microsoft.Extensions.DependencyInjection
{
    public static class CustomConfigurationClass
    {
        public static IServiceCollection AddNameOfService(this IServiceCollection services)
        {
            // Add services to the IServiceCollection
            // ...
            return services;
        }
    }
}
```

## Injection

Handled by the `IServiceProvider` or `ActivateUtilities`. 

## Injecting into Constructors

### Rules 
Arguments that cannot be resolved by the `IServiceProvider` must have a default value.  

Constructors must be `public`.  

Only one constructor can be fullfilled by the `ActivatorUtilities`.  

The `IServiceProvider` can resolve depdencies for classes with overloaded constructors.  The constructor with the greatest number of dependencies that can be resolved will be selected and used by the `IServiceProvider` to instantiate the object.

## Injecting into Action Methods

```csharp
public IActionResult ActionMethod([FromServices] IServiceInterface service)
{
    // ...
    service.DoSomething()
    // ...
}
```

## Injecting into Middleware

Middleware components are resolved when the application starts, effectively making them singletons.  Therefore, services with a singleton liftetime are the only type of service that may be injected into a middleware constructors.

Since middleware is effectively a singleton, it may not encapsulate services with transient or scoped lifetimes.   To inject services with transient or scoped lifetimes, add parameters to the `Invoke()` or `InvokeAsync()` method, which are executed for each HTTP request.

```csharp
public void Invoke(HttpContext context, IServiceInterface service)
{
    // ...
    service.DoSomething();
    // ...
}
```

## Scan Assemblies to Automatically Add Services to `IServiceCollection`

Add a reference to the `Scutor` NuGet package.

Implement marker interfaces on the `IServiceInterface`.

```csharp
interface ISingletonServiceInterface : IServiceInterface {}
interface IScopedServiceInterface : IServiceInterface {}
```

Use `Scrutor` `Scan()` extension method on the `IServiceCollection`.

```csharp
services.Scan(scan => {
    scan.FromAssemblyOf<IServiceInterface>()
        .AddClasses(c = > c.AssignableTo<ISingletonServiceInterface>())
            .As<IServiceInterface>()
            .WithSingletonLifetime()
        .AddClasses(c = > c.AssignableTo<IScopedServiceInterface>())
            .As<IServiceInterface>()
            .WithScopedLifetime()
});
```

## Applying the Decorator Pattern to a Service

Apply the Decorator Pattern
```csharp
public class Decorated : IDecorator
{
    private readonly IDecorator _decorator;

    public Decorated(IDecorator decorator)
    {
        _decorator = decorator;
    }
}
```

Use `Scrutor` `Decorate()` extension method to add decorated service to `IServiceCollection`
```csharp
services.Decorate<IDecorator, Decorated>();
```