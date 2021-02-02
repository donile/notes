# Binding Configuration Settings to a Class

`appsettings.json`
```json
{
    "Section":
    {
        "MyProperty": true,
        "MyOtherProperty": false
    }
}
```

```csharp
public class MyConfiguration
{
    public bool MyProperty { get; set; }
    public bool MyOtherProperty { get; set; }
}
```

```csharp
var myConfiguration = new MyConfiguration();
_configuration.Bind("Section", myConfiguration);

// myConfiguration.MyProperty == true  => true
// myConfiguration.MyOtherProperty == false => true
```
