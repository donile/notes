# Including Related Entities in Response

## Use the `$expand` Query Parameter

`http://my.domain.com/odata/Items?$expand=NavigationPropertyA,NavigationPropertyB`

`http://my.domain.com/odata/Items?$expand=NavigationProperty($expand=NavigationPropertyB)`

`http://my.domain.com/odata/Items?$select=PropertyA&expand=NavigationProperty`

`http://my.domain.com/odata/Items?$select=PropertyA&expand=NavigationProperty($select=PropertyA;$expand=NavigationPropertyB)`


## Maximum Expansion Depth

Limit the depth of related entities that will be included in the response by setting the `MaxExpansionDepth` property on the `EnableQuery` attribute.

This configures the validation of the query.

A `400 Bad Request` will be returned if the depth of navigation properties requested exceeds the `MaxExpansionDepth`.

```csharp
[HttpGet("Items")]
[EnableQuery(MaxExpansionDepth = MAX_EXPANSION_DEPTH)]
public IActionResult GetItems()
{
    ...
}
```
