# Options Pattern
Injection and Consumption of Strongly typed options

## Configuring Options

```csharp
public class StartUp
{
    private readonly IConfiguration _configuration;

    public StartUp(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public ConfigureServices(IServiceCollection services)
    {
        // bind the configuration in "Section" to IOptions<MyConfiguration>
        services.Configure<MyConfiguration>(_configuration.GetSection("Section"));
    }
}
```

## Accessing Configuration through an `IOptions<T>` Instance
Requires configuration to be added to `IServiceCollection` in `StartUp.ConfigureServices()`.

```csharp
using Microsoft.Extensions.Options;

public class MyService
{
    private readonly MyConfiguration _myConfiguration;

    public MyConstructor(IOptions<MyConfiguration> options)
    {
        _myConfiguration = options.Value;
    }
}
```

## Automatically Reload Options Per HTTP Request
`IOptionsSnapshot<T>` registered as a **scoped** service.  Each time a HTTP request is received, the configuration file will be read and the type `T` provided by the `IOptionsSnapshot<T>` instance will have the latest configuration settings.

Since `IOptionsSnapshot<T>` is added to the `IServiceCollection` as a service with a scoped lifetime, it should not be used as a dependency of a service with a singleton lifetime.  By default, the dependency injection service will throw an exception if a service with a singleton lifetime attempts to have a dependency with a scoped lifetime injected.

```csharp
using Microsoft.Extensions.Options;

public class MyService
{
    private readonly MyConfiguration _myConfiguration;

    public MyConstructor(IOptionsSnapshot<MyConfiguration> options)
    {
        _myConfiguration = options.Value;
    }
}
```

## Autmoatically Reload Options with `IOptionsMonitor<T>`
Added as a singleton to the `IServiceCollection`.

```csharp
using Microsoft.Extensions.Options;

public class MyService
{
    private readonly IOptionsMonitor<MyConfiguration> _options;

    private readonly MyConfiguration _myConfiguration;

    private bool MySetting { get => _myConfiguration.Mysetting }

    public MyConstructor(IOptionsMonitor<MyConfiguration> options)
    {
        _options = options;

        // register a listener to update the configuration object when the configuration file changes
        options.OnChange(configuration => {
            _myConfiguration = configuration;
        });
    }
}
```
