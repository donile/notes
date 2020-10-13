# Configure a Mock Object to Raise Events

## Raising an Event Manually Within a Test
```csharp
var mock = Mock<Interface>();
mock.Raise(x => x.Event += null,
           new EventArgs());
```

## Raising an Event Automatically After a Method Invocation
```csharp
var mock = Mock<Interface>();
mock.Setup(x => x.Method())
    .Raises(x => x.Event += null,
            new EventArgs());
```