# Options Validation

## Validate with Data Annotations

## Validate with Delegates
`StartUp` class
```csharp
// injected into and set by the StartUp constructor
private readonly IConfiguration _configuration;

//...

public void ConfigurServices(IServiceCollection services)
{
    // ...

    services.AddOptions<MyConfiguration>()
        .Bind(_configuration.GetSection("MyConfiguration"))
        .Validate(cfg => {
            // perform validation of cfg and return boolean
            // to indicate if MyConfiguration is valid
        });
    
    // ...    
}
```

## Validate with `IValidateOptions<T>`
All instances of `IValidateOptions<T>` added to `IServiceCollection` will be executed

Can use services added to `IServiceCollection`

Implement `IValidateOptions<T>` on an options validator class
```csharp
public class MyConfigurationValidator : IValidateOptions<MyConfiguration>
{
    private readonly IService _service; 
    
    public MyConfigurationValidator(IService service)
    {
        _service = service;
    }

    public ValidateOptionsResult Validate(string name, MyConfiguration config)
    {
        if (_service.ReturnsTrueFor(config.Setting))
        {
            return ValidateOptionsResult.Fail("With this failure message");
        }

        return ValidateOptionsResult.Success;
    }
}
```

Add `IValidateOptions<T>` to `IServiceCollection` in `StartUp.ConfigureServices()`
```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.TryAddEnumerable(ServiceDescriptor.Singleton<IValidateOptions<MyConfiguration>, MyConfigurationValidator>());
}
```

## Validate Named Options with `IValidateOptions<T>`
Implement `IValidateOptions<T>` on an options validator class
```csharp
public class MyConfigurationValidator : IValidateOptions<MyConfiguration>
{
    private readonly IService _service; 
    
    public MyConfigurationValidator(IService service)
    {
        _service = service;
    }

    public ValidateOptionsResult Validate(string name, MyConfiguration config)
    {
        switch(name)
        {
            case "NamedOption":
                if (_service.ReturnsTrueFor(config.Setting))
                {
                    return ValidateOptionsResult.Fail("With this failure message");
                }

                return ValidateOptionsResult.Success;

            case default:
                return ValidateOptionsResult.Skip;
        }        
    }
}
```