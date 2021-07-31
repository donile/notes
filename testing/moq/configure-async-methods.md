# Configuring a Mock Object's Methods

## Configure Mock Object's Async Method to Return Null

Passing `null` as the value parameter to the `ReturnsAsync()` method will not compile because the `ReturnsAsync()` is overloaded to accept a `TResult` parameter or a `Func<TResult>` and the compiler cannot determine which `ReturnsAsync()` method to execute.  Passing a lambda function into the `ReturnsAsync()` method disambiguates the type of the argument passed to the `ReturnsAsync()` method and returns the required `null` value from the mocked `async` method. <sup>[1]</sup>

```csharp
mock.Setup(interface => inteface.Method())
    .ReturnsAsync(() => null));
```

Casting `null` value to the mocked method's return type also disambiguates which `ReturnsAsync()` method should be executed.

```csharp
mock.Setup(interface => inteface.Method())
    .ReturnsAsync((ResultType)null));
```

[1]: https://stackoverflow.com/questions/41636077/returnsasyncnull-creates-a-build-error-when-using-moq-for-unit-testing-in-vs15