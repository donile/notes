# ASP.NET Core Configuration

# Using the `IConfiguration` Interface

## Accessing Configuration
Get an `IConfiguration` from the dependency injection container.

```csharp
using Microsoft.Extensions.Configuration;

private readonly IConfiguration _configuration;

public MyObjectConstructor(IConfiguration configuration)
{
    _configuration = configuration;
}
```

## Accessing a Configuration Setting

```csharp
private readonly IConfiguration _configuration;
...
TSetting setting = _configuration.GetValue<TSetting>("Section:NestSection:Key");
```

## Accessing a Configuration Section
```csharp
IConfigurationSection section = _configuration.GetSection("Section");
TSetting setting = section.GetValue<TSetting>("Key")
```

## Accessing a Configuration Setting That Is Not Present
If the key for a configuration setting is not present, the default value for the type of value trying to be retrieved will be returned.

```csharp
// setting will contain the default value for an instance of TSetting
TSetting setting = _configuration.GetValue<TSetting>("This:Key:Is:Not:Present");
```
