# Derived Types

Restrict results to those of the derived type.

## Implement OData API Endpoint for Derived Types

```csharp
[HttpGet("Items/MyNameSpace.ThingDerivedFromItem")]
public async Task<IActionResult> GetThingsDerivedFromItemClass()
{
    ...
}
```

## Querying for a Derived Type

`http://my.domain.com/Items/MyNameSpace.ThingDerivedFromItem`
