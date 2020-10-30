# AutoFixture: Configure Object Creation

## Configure Anonymous Return Results

Return the same result each time a type T is requested.
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    fixture.Inject(new T());
    var obj = fixture.Create<T>();
}
```

## Configure the Creation Algorithm
Call the lambda function and return it's result 
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    fixture.Register(() => return new T());
    var obj = fixture.Create<T>(); // will call lambda passed to Register()
}
```

## Freezing Return Results
Return the same result each time a type T is requested.
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    var obj = fixture.Freeze<T>();
}
```