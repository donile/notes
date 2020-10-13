# Configure a Partial Mock Object
Partial mock object's use some of the functionality provided by a class and replace some of it's members with mocked implementations.

## Configuring a Partial Mock Object's Method
Requires `MockedMethod` to be `public` and `virtual`.
```csharp
var mock = Mock<Class>();
mock.Setup(x => MockedMethod())
    .Returns(returnValue);
```

## Configuring a Partial Object's Protected Method
Requires `MockedMethod` to be `protected`.
```csharp
using Moq.Protected;
...
var mock = Mock<Class>();
mock.Protected().Setup(<TReturn>("ProtectedMemberName")
              .Returns(returnValue);
```

## Configuring a Partial Object's Protected Method Including Types
Define an interface for the protected members of the class being mocked
```csharp
using Moq.Protected;
...

interface IProtectedMembersInterface
{
    bool ProtectedMethod();
}

...
var mock = Mock<Class>();
mock.Protected()
    .As<IProtectedMembersInterface>()
    .Setup(x => ProtectedMethod())
    .Returns(true);
```

## An Alternative to Partial Mocks
Refactor the class so it's constructor accepts an interface that exposes the minimum amount of functionality required to eliminate the need for the partial mock's method/property invocation.