# Configuration Providers
Read configuration from different types of sources  
Load configuration for an application  
Executed in the order they are added to the `IConfigurationBuilder`  
Key-value pairs are provided or replaced by each `IConfigurationProvider` added to the `IConfigurationBuilder` 
The order of in which `IConfigurationProvider`s are added to the `IConfigurationBuilder` is important  
The last value read from an `IConfigurationProvider` is the value that is supplied to the application  

## Default Configuration Providers
Several `IConfigurationProvider`s are added during execution of `Host.CreateDefaultBuilder()`  
`Host.CreateDefaultBuilder()` executes `IHostBuilder.ConfigureAppConfiguration()`  
`IHostBuilder.ConfigureAppConfiguration()` adds default `IConfigurationProvider`s  

## Configuration Types
Host Configuration  
Application Configuration (setup by `IConfigurationProvider`s)

## Default Configuration Providers
JSON File(s)  
dotnet User Secrets  
Environment Variables  
Command Line Args  

### Psuedo Code Showing the Order in which the Default `IConfigurationProviders` are Added
The last `IConfigurationProvided` added to the `IConfigurationBuilder` supplies the configuration value when requested by the application.

```csharp
public static IHostBuilder.ConfigureAppConfiguration((hostingContext, configBuilder)
    ...
    configBuilder.AddJsonFile();
    configBuilder.AddUserSecrets();
    configBuilder.AddEnvironmentVariables();
    configBuilder.AddCommandLine(args);
    ...
)
```

## `IHostBuilder.ConfigureAppConfiguration()` Extension Method
Adds a new `IConfigurationProvider` to the list of existing `IConfigurationProvider`s.  

```csharp
public class Program
{
    ...
    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder => {
                webBuilder.UseStartUp<StartUp>();
            })
            .ConfigureAppConfiguration((hostingContext, configurationBuilder) => {
                
                // add custom IConfigurationProvider here
                configurationBuilder.Add(new MyConfigurationProvider());
            });
    }
    ...
}
```

## Customize the Order in which `IConfigurationProvider`s are Added to the `IConfigurationBuilder`
Clear the list of `IConfigurationProvider`s  
Register new `IConfigurationProvider`s 

```csharp
public class Program
{
    ...

    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder => {
                webBuilder.UseStartUp<StartUp>();
            })
            .ConfigureAppConfiguration((hostingContext, configurationBuilder) => {
                
                // clear list of IConfigurationProviders previously added
                configurationBuilder.Sources.Clear();

                // register configuration providers in customized order
                configurationBuilder.Add(new MyConfigurationProvider());
                ...
            })
    }
    ...
}
```

## Create a Custom `IConfigurationProvider`
Extend abstract class `ConfigurationProvider`  
Add a constructor to `ConfigurationProvider` that accepts an `Action<MyOptionsBuilder>` parameter  
Override the `Load()` method  
Create an instance of `MyOptionsBuilder` in `Load()` method  
Pass `MyOptionsBuilder` to `Action<MyOptionsBuilder>`  
Build and return options by calling `MyOptionsBuilder.Options` property  
Set the `ConfigurationProvider.Data` property within the overridden `Load()` method  

```csharp
public class MyConfigurationProvider : ConfigurationProvider
{    
    public MyConfigurationProvider(Action<MyOptionsBuilder> configureAction)
    {
        ConfigureAction = configureAction;
    }

    private Action<MyOptionsBuilder> ConfigureAction { get; }

    public override Load()
    {
        var myOptionsBuilder = new MyOptionsBuilder();

        ConfigureAction(myOptionsBuilder);

        var myDependency = new MyDepdendency(myOptionsBuilder.Build()) 

        Data = myDependency.ProvideConfigurationAsDictionary();
    }
}
```

### Define an `IConfigurationSource`  
Creates an instance of an `IConfigurationProvider`  
Implements `IConfigurationSource` interface  
`IConfigurationSourceBuild()` method returns an instance of the previously created `MyConfigurationProvider`, which is an instance of an `IConfigurationProvider`.  

```csharp
public class MyConfigurationSource : IConfigurationSource
{
    private readonly Action<MyOptionsBuilder> _configureAction;
    
    public MyConfigurationSource(Action<MyOptionsBuilder> configureAction)
    {
        _configureAction = configureAction;
    }

    public IConfigurationProvider Build()
    {
        return new MyConfigurationProvider(_configureAction);
    }
}
```

### Create an `IConfigurationBuilder` extension method
Extension method accepts an `Action<MyOptionsBuilder>` parameter  
Extension method calls `IConfigurationBuilder.Add()` to add the new `IConfigurationSource`   

```csharp
public static class MyConfigurationExtensions
{
    public static IConfigurationBuilder AddMyConfiguration(
        this IConfigurationBuilder builder, 
        Action<MyOptionsBuilder> configure
    )
    {
        return builder.Add(new MyConfigurationSource(configure));
    }
}
```

### Register the `IConfigurationProvider` 
Use the previously created `IConfigurationBuilder.AddMyConfiguration()` extension method to add the `IConfigurationSource` to the `IConfigurationBuilder`.

```csharp
public class Program
{
    ...
    public static IHostBuilder CreateHostBuilder(string[] args)
    {
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder => {
                webBuilder.UseStartUp<StartUp>();
            })
            .ConfigureAppConfiguration((hostingContext, configurationBuilder) => {
                
                // get temporary instance of configuration that falls out of scope
                // when the method ends
                var config = configurationBuilder.Build();

                // call custom extension method on configurationBuilder
                configurationBuilder.AddMyConfiguration(myOptions => {
                    myOptions.ConfigureStuff(config["configKey1"]);
                    myOptions.ConfigureMoreStuff(config["configKey2"]);
                })
            })
    }
    ...
}
```
