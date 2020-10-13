# Configuring a Mock Object's Methods

## Mocking an Interface

```csharp
var mock = new Mock<IInterface>();
var mockedObject = mock.Object;
```

## Configure Mock's Method to Return a Specific Result
```csharp
mock.Setup(interface => interface.Method())
    .Returns(true));
```

## Configure Mock's Method to Return Result for Specific Argument Values
```csharp
mock.Setup(interface => interface.Method("arg1", 1))
    .Returns(true));
```

## Configure Mock's Method to Return Result for Specific Subset of Argument Values
```csharp
mock.Setup(interface => interface.Method(It.IsAny<string>, 1))
    .Returns(true));
```

## Configure Mock's Method to Return Output Parameter
```csharp
bool outParameter = true;
mock.Setup(interface => interface.Method(out outParameter));
```

## Configure Mock's Method to Return Ref Parameters
```csharp
delegate void Callback(ref parameter);

mock.Setup(
        interface => interface.Method(ref It.Ref<IdentityVerificationStatus>.IsAny))
    .Callback(new Callback((ref parameter => parameter)));
```

## Configure Mock Object's Method to Return Null
Any mock object's method that returns a reference type will return `null` by default, or it can be explicitly set to return `null` as follows.
```csharp
mock.Setup(interface => inteface.Method())
    .Returns<T>(null);
```