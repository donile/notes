# AutoFixture.AutoMoq

## Inject Mocked Dependency Into Object Instantiated by AutoFixture

Using AutoFixture.AutoMoq to instantiate the system under test (SUT) provides several benefits, most notably of which is that it is not necessary to update the calls to the SUT's constructor each time the SUT's constructor signature changes.  Instead, `freeze` the mock interface on the `Fixture` used to instantiate the SUT, instructing the `Fixture` to create the SUT with the same mock object each time the SUT is instantiated.

```csharp
// requires Nuget package AutoFixture.AutoMoq
using AutoFixture.AutoMoq;

[Test]
public void TestMethod()
{
    var fixture = Fixture();
    fixture.Customize(new AutoMoqCustomization());

    var mockDependency = fixture.Freeze<Mock<IDependency>>();

    // AutoMoqCustomization will create mock of interface the thing depends on
    var thing = fixture.Create<ClassThatDependsUponInterface>();

    mockDependency.Verify(x => x.Method(), Times.Once());
}
```

## Parameterize Unit Tests With Object's That Need Mocked Dependencies

Create an attribute that populates test method parameters
```csharp
// The AutoFixture.AutoMoq namespace is in AutoFixture.AutoMoq Nuget package
// The AutoFixture.NUnit3 namespace is in AutoFixture.NUnit3 Nuget package
using AutoFixture;
using AutoFixture.AutoMoq;
using AutoFixture.NUnit3;
using System;

[AttributeUsage(AttributeTargets.Method)]
public class AutoMoqDataAttribute : AutoDataAttribute
{
    public AutoMoqDataAttribute()
        : base(() => new Fixture().Customize(new AutoMoqCustomization()))
    {

    }
}
```

Apply the `AutoMoqData` attribute created above to the test method.  The attribute will populate the test method arguments.

Apply the `Frozen` attribute to the `mockDependency` parameter **BEFORE** the system under test parameter that requires the dependency.

```csharp
[Test]
[AutoMoqData]
public void TestMethod(int arg, [Frozen] TDependency mockDependency, T sut)
{
    sut.DoSomethingThatRequiresDependency();

    mockDependency.Verify(x => x.Method(), Times.Once());
}
```
