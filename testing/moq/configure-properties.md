# Configuring a Mock Object's Properties

## Configure a Mock Object's Property to Return a Specific Result
```csharp
var mock = new Mock<Interface>();
mock.Setup(x => x.Property)
    .Returns(1);
```

## Configure a Mock Object's Hierarchy of Properties
```csharp
var mockChild = new Mock<ChildInterface>();
mockChild
    .Setup(x => x.Property)
    .Returns(1);

var mockParent = new Mock<ParentInteface>();
mockParent
    .Setup(x => x.ChildProperty)
    .Returns(mockChild.Object);
```

## Configure a Mock Object's Hierarchy of Properties Automatically
Properties must be virtual
```csharp
var mock = new Mock<Interface>();
mock.Setup(x => x.ChildProperty.Property)
    .Returns(1);
```

## Configure a Mock Object's Property to Track Changes
```csharp
var mock = new Mock<Interface>();
mock.SetupProperty(x => x.Property, initialValue);
```

## Configure Change Tracking on All of a Mock Object's Properties
Always call `SetupAllProperties()` before setting individual property return values.
```csharp
var mock = new Mock<Interface>();
mock.SetupAllProperties();
mock.Setup(x => x.Property)
    .Returns(value);
```