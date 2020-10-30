# AutoFixture: TestDataBuilder

## Omit Specific Properties
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var obj = fixture.Build<T>()
                .Without(x => x.PropertyToOmit)
                .Create();
}
```

## Omit All Properties
Set all properties to their default values
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var obj = fixture.Build<T>()
                .OmitAutoProperties()
                .Create();
}
```

## Set Property to Specific Value
```csharp
[Test]
public void TestMethod()
{
    var value = new T();
    var fixture = new Fixture();
    var obj = fixture.Build<T>()
                .With(x => x.Property, value)
                .Create();
}
```

## Perform a Specified Action on the Specimen
```csharp
[Test]
public void TestMethod()
{
    var item = new T();
    var fixture = new Fixture();
    var obj = fixture.Build<T>()
                .Do(x => x.CollectionProperty.Add(item))
                .Create();
}
```

## Specify Creation Algorithm for Type T
Provides the `Fixture` instance with a builder configured to return an instance of type T, but it does **NOT** create the instance.
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var obj = fixture.Build<T>(builder =>
        builder
            .With(x => x.Property, value)
            .Without(x => x.Property)
            .Do(x => x.Collection.Add(item)));
}
```