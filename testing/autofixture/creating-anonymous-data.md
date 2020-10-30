# AutoFixture: Creating Anonymous Data

## Create an Anonymous Char
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<char>();
    ...
}
```

## Create an Anonymous String
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<string>();
    ...
}
```

## Create an Anonymous String with Prepended Seed Value
```csharp
// requires Nuget package AutoFixture.SeedExtensions

[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<string>("PrependedSeedString");
    ...
}
```

## Create an Anonymous Integer
AutoFixture will provide an integer greater than zero by default.
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<int>();  // return int greater than zero
    ...
}
```

## Create an Anonymous Decimal
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<decimal>();
    ...
}
```

## Create an Anonymous DateTime
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<DateTime>();
    ...
}
```

## Create an Anonymous Guid
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<Guid>();
    ...
}
```

## Create an Anonymous Guid
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<TEnum>();
    ...
}
```

## Create an Anonymous Sequence
AutoFixture creates a sequence with 3 items of type T by default
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.CreateMany<T>();
    ...
}
```

## Create an Anonymous Sequence with a Specific Number of Items
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.CreateMany<T>(itemCount);
    ...
}
```

## Add Anonymouse Data Items to an Existing List
AutoFixture adds 3 items of type T by default
```csharp
[Test]
public void TestMethod()
{
    var collection = new List<T>();
    ...

    var fixture = new Fixture();
    var value = fixture.AddManyTo(collection);
    ...
}
```

## Add Specific Number of Anonymouse Data Items to an Existing List
AutoFixture adds 3 items of type T by default
```csharp
[Test]
public void TestMethod()
{
    var collection = new List<T>();
    ...

    var fixture = new Fixture();
    var value = fixture.AddManyTo(collection, itemsToAddCount);
    ...
}
```

## Create an Anonymous Object of Type T
AutoFixture will populate the properties of the instance of type T
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var value = fixture.Create<T>();
    ...
}
```

