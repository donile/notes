# Named Options
Supported by `IOptionsMonitor<T>` and `IOptionsSnapshot<T>`  
Bind settings to the same class for different logical options  

`appsettings.json`
```json
{
    "Api1":
    {
        "Url": "http://api1.com/get-info"
    },
    "Api2":
    {
        "Url": "http://api2.com/get-info"
    }
}
```

`ApiConfig` class
```csharp
public class ApiConfig
{
    public const string Api1 = "Api1";
    public const string Api2 = "Api2";

    public string Url { get; set; }
}
```

`StartUp` class
```csharp
// injected into and set by the StartUp constructor
private readonly IConfiguration _configuration;
...
public ConfigureServices(IServiceCollection services)
{
    services.Configure<ApiConfig>(Api1, _configuration.GetSection(Api1));
    services.Configure<ApiConfig>(Api2, _configuration.GetSection(Api2));
}
...
```

`DependentService` class
```csharp
public class DependentService
{
    private readonly ApiConfig _apiConfig;

    public DependentService(IOptionsMonitor<ApiConfig> options)
    {
        _apiConfig = options.Get(Api1);
    }
}
```