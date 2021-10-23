# AutoFixture: Specimens

**Specimen** An individual value for a specific data type used as an exmaple of that data type.

## Specimen Examples

string specimen: "the quick brown fox"

integer specimens: 6, 23, 90

`Fixture`s are provided default `ISpecimenBuilder` instances which may be replaced by adding pre-built or custom built `ISpecimenBuilder` instances to the `Fixture`'s `Customizations` collection.

## AutoFixture Pipeline Stages
* Customizations
* Engine
* Residue Collectors

When a type is created by a `Fixture` instance, it proceeds through the three stages of the pipeline until an `ISpecimenBuilder` can return the result.

### `Customizations`
* Collection of `ISpecimenBuilder`s
* Replace default `ISpecimenBuilder`s configured for a `Fixture`
* Add prebuilt or custom `ISpecimenBuilder` instances to `Fixture`'s `Customizations` collection

### Engine
* Preconfigured in a `Fixture` instance
* Example: `GuidGenerator` specimen builder returns Guids

### Residue Collectors
* Lowest priority
* Fallback if no other matches

## Add a Customization to a Fixture
Pass an instance of an object that implements the `ICustomization` interface to the `Fixture` instance's `Customize()` method.
```csharp
[Test]
public void TestMethod()
{
    var fixture = new Fixture();
    fixture.Customize(new DateTimeCustomization()); // DateTimeCustomization : ICustomization

    var dt = fixture.Create<DateTime>(); // will use DateTimeCustomization to generate DateTime
    
}
```

## Create a Customization
Implement the `ISpecimenBuilder` interface and add instance to `Fixture`'s `Customizations` collection.
```csharp
using AutoFixture.Kernel;

public class AnonymousDataGenerator : ISpecimenBuilder
{
    public object Create(object request, ISpecimenContext context)
    {
        if (!ShouldRespondTo(request))
        {
            // null is a valid specimen to return, so to signal that this
            // ISpecimenBuilder could not handle the request, return an instance ;// of NoSpecimen
            return new NoSpecimen();
        }

        return new AnonymousData();
    }

    private bool ShouldRespondTo(request)
    {
        // determine if the incoming request should be handled by this
        // ISpecimenBuilder
        return typeof(request).Equals(TypeOfAnonymousDataToReturn);
    }
}
```