# Configure a Mock Object to Throw Exceptions

## Throwing an Exception from a Mock Object
```csharp
var mock = Mock<Interface>();
mock.Setup(x => x.Method())
    .Throws<TException>();
```

## Throwing an Exception That Has Been Configured
```csharp
var mock = Mock<Interface>();
mock.Setup(x => x.Method())
    .Throws(new Exception("message"));
```