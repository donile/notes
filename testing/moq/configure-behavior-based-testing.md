# Configure Behavior Based Testing

Query the mock object to verify methods were executed and/or property values were set or retrieved.

## Verify a Method Without Parameters is Invoked
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.Verify(x => x.Method());
```

## Verify a Method With Specific Argument Values is Invoked
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.Verify(x => x.Method(arg1, arg2));
```

## Verify a Method With a Specific Subset of Arguments is Invoked
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.Verify(x => x.Method(It.IsAny<string>(), It.IsAny<int>()));
```

## Verify a Method is Inovked a Specific Number of Times
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.Verify(
    x => x.Method(It.IsAny<string>(), It.IsAny<int>()),
    Times.Between(0, 2));
```

## Verify a Property's Value is Read
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.VerifyGet(x => x.Nested.Property);
```

## Verify a Property's Value is Set to a Specific Value
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.VerifySet(x => x.Nested.Property = expectedValue);
```

## Verify a Property's Value is Set to a Specific Value
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.VerifySet(x => x.Nested.Property = expectedValue);
```

## Verify a Property's Value is Set a Specific Number of Times
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.VerifySet(x => x.Nested.Property = expectedValue, Times.Once);
```

## Verify No Other Methods Inovked or Properties Read/Set
```csharp
var mock = new Mock<Interface>();
...
// execute act phase of test
...
mock.VerifyNoOtherCalls();
```