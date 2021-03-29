# Log Levels

## Trace/Verbose

## Debug

## Information

## Warning

## Error

## Critical/Fatal


## Categories

## Events

## Get ILogger Instance
Use the Dependency Injection container to inject an instance of an `ILogger<T>`.  The type `T` is considered the Category for the `ILogger` instance.

```csharp
public class MyController : Controller
{
    private readonly ILogger<MyController> _logger;

    public MyController(ILogger<MyController> logger)
    {
        _logger = logger;
    }
}
```

## Create ILogger from ILoggerFactory
Using the `ILoggerFactory` to create an `ILogger` instance allows for the Category to be set with a string instead of the type parameter.  When an `ILogger<T>` is instantiated, it's Category is set to the name of the class of type `T`.  The `string` passed to the `ILoggerFactory.Create()` method sets the Category for the `ILogger` instance returned.

```csharp
public class MyController : Controller
{
    private readonly ILogger<MyController> _logger;

    public MyController(ILoggerFactory loggerFactory)
    {
        _logger = loggerFactory.Create("MyCategory");
    }
}
```

## Create a Log Entry

```csharp
_logger.LogInformation("Logging {myObject}", myObject);
```

## Filters

