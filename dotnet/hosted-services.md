# Hosted Services

.NET Generic Host

Cross Cutting Concerns
* Logging
* Configuration
* Dependency Injection

Types of Services
* Windows Services
* Linux Workers
* Long-running console app

`IHostedService` Abstracts away the differences between the different types of services

* Graceful start and shutdown
* Cross-platform
* Same model to host ASP.NET applications

Woker Service Template

`IHostBuilder.CreateDefaultBuilder()`
* Sets up default for logging and configuration


Register the IHostedService
```csharp
IHostBuilder.ConfigureServices((host, services) => {
  services.AddHostedService<Worker>();
});
```


Entry Point for the Service
```csharp
BackgroundService.ExecuteAsync(CancellationToken token)
```

There are different extensions methods if different NuGet packages that can be used to setup the lifetime of the IHost.

IHostBuilder.UseConsoleLifetime()
IHostBuilder.UseWindowsService()
IHostBuilder.UseSystemD()
