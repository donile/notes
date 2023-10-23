# `IDisposable`

Source: Plurasight Course '`IDisposable` Best Practices for C# Developers'

`IDisposable` is not tied to the garbage collector (GC) mechanism.

The garbage collector does not call `Dispose()`.

Provides a mechanism for releasing unmanaged resources.

Performs application-defined tasks associated with freeing, releasing or resetting unmanaged resources.

## Use Case

### Database Connection Exhaustion

Dispose of an object with a database connection, allowing the connection to return to the connection pool.  If the object is not disposed after use, the connection will remain open.  In an environment where many connections may be made concurrently, this could cause the connections available in the connection pool to be exhausted before the garbage collector removes the object from the heap.

## Unamaged Resources

File (`Stream`)

Database (`DbConnection`)

HTTP (`HttpClient`)

## `using` blocks

When the `using` block ends, `Dispose()` is called.

```csharp
using (var obj = new DisplableClass())
{
  // do stuff with obj...
}
```

`Dispose()` is called when the code exits the current block

```csharp
for(var i = 0; i < 10; i++)
{
  using var obj = new DisposableClass();
  // call obj.Dispose() 10 times
}
```

The `using` block is functionally equivalent to wrapping the code in a `try`/`finally` block.

```csharp
var obj = new DisposableClass();
try
{
  // do stuff with obj...
}
finally
{
    obj.Dispose();
}
```

## Best Practices

Dispose of `IDisposable` objects as soon as possible

Implement `IDisposable` when a class' fields use `IDisposable` objects

Dispose of all `IDisposable` objects used in a class' fields

Allow `Dispose()` to be called multiple times 

Don't throw exceptions within `Dispose()`

Dispose of `IServiceScope`

Use `IHttpClientFactory` to manage `HttpClient` lifetime

`IHttpClientFactory` implmenetations will re-use `HttpClient` objects in a thread-safe manner

## Garbage Collector

Does not call `Dispose()`

## Dispose Pattern

Calling `GC.SuppressFinalize(this)` informs the garbage collector that the object has cleaned up all of it's own resources.

`Dispose()` should not throw any exceptions

`Dispose()` should be callable multiple times without failures

```csharp
public void Dispose()
{
  Dispose(true);
  GC.SuppressFinalize(this);
}

protected virtual void Dispose(bool disposing)
{
  if (_disposed)
  {
    return;
  }

  if (disposing)
  {
    // dispose managed resources (e.g. SqlConnection)
    if (_disposableField != null)
    {
      _disposableField.Dispose();
      _disposableField = null;
    }
  }

  // dispose unmanaged resources (e.g. memory)...
  
  _disposed = true;
}
```

## Garbage Collector Finalizers

Finalizers are executed once by the garbage collector

Finalizers should be implemented by any class that implements `IDisposable`

Finalizers should clean up unmanaged resources in the event the user of the class forgets to call `Dispose()`

Calling `GC.SuppressFinalize(this)` in `Dispose()` prevents the garbage collector from calling the finalizer method

Managed resources, such as a `DbConnection` or `Stream`, may have already been disposed so they should not be disposed again by a finalizer.

Unmanaged resources, such as allocated memory, should always be disposed of by the finalizer.

```csharp
~ClassName()
{
  Dispose(false);
}
```

## Static Code Analysis

Enable static code analysis in the `.csproj` file.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <AnalysisMode>AllEnabledByDefault</AnalysisMode>
    ...
  </PropertyGroup>
  ...
</Project>
```

Review compiler warnings for code `CA2000`.


## Object Lifetime with Dependency Injection

Creating a scope instantiates an entire object graph.

Dispose of the scope when its no longer needed.

```csharp
using var scope = services.CreateScope();
{
  // use scope
}
```

## `IAsyncDisposable`

Implement `IAsyncDisposable`

```csharp
public async ValueTask DisposeAsync()
{
  await DisposeAsyncCore();
  GC.SuppressFinalize(this);
}

protected virtual async ValueTask DisposeAsyncCore()
{
  if (_asyncDisposableField is not null)
  {
    await _asyncDisposableField.DisposeAsync().ConfigureAwait(false);
    _asyncDisposableField = null;
  }
}
```

Use `await using`

```csharp
await using var asyncDisposableObject = new AsyncDisposableClass();
```
