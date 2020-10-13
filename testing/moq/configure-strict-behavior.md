# Configurating Mock Objects to Use Strict Behavior

## Strict versus Loose Mocks

Loose mocks are created by default.

A loose mock:

Returns default values
    
Does NOT throw excpetions

Does NOT require methods to be explicitly configured

## Creating a Strict Mock

```csharp
var mock = Mock<Interface>(MockBehavior.Strict);
```