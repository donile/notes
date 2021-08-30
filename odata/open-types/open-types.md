# Open Types

An entity or complex type that persists undeclared properties.

## Implement `DynamicProperty` Helper Class

Composite key consists of `MyOpenTypeClassId` and `Key` because two different instances of `MyOpenTypeClass` could have a dynamic property with the same `Key`.

If using EF Core, a composite key must be added using the fluent API.

```csharp
...
using System.ComponentModel.DataAnnotations.Schema;
...

[Table("MyOpenTypeClassDynamicProperties")]
public class DynamicProperty
{
    public string Key { get; set; }
    public string SerializedValue { get; set; }
    public Guid MyOpenTypeClassId { get; set; }
    public MyOpenTypeClass MyOpenType { get; set; }

    // prevents EF Core from attempting to store property in database
    [NotMapped]
    public object Value
    {
        get
        {
            return JsonConvert.DeserializeObject(SerializedValue);
        }

        set
        {
            SerializedValue = JsonConvert.SerializeObject(value);
        }
    }
}
```

## Model Implementation

Implement a property of type `Dictionary<string, object>`

The `EntityType` element in the OData metadata document will contain an `OpenType="true"` attribute.

```csharp
public class MyOpenTypeClass
{
    ...
    private readonly IDictionary<string, object> _properties;

    public ICollection<DynamicProperty> DynamicProperties { get; set; }
    
    [NotMapped]
    public IDictionary<string, object> Properties
    {
        get
        {
            foreach (var property in DynamicProperties)
            {
                _properties.Add(property.Key, property.Value);
            }
        }
        
        set
        {
            _properties = value;
        }
    }
    ...

    public MyOpenTypeClass()
    {
        _properties = new Dictionary<string, object>();
        DynamicProperties = new List<DynamicProperties>();
    }
}
```
