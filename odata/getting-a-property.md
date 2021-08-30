# Getting a Property

## Standards

If property is not defined for entity, return 404 Not Found.

If property is found but null, return 204 No Content.

## Example URIs

### GET Property

`GET http://hostname:port/root/EntitySet(key)/PropertyName`

#### Returns Property Value Wrapped in JSON

```json
{
    "value": <property-value>
}
```

### GET Property Value

`GET http://hostname:port/root/EntitySet(key)/PropertyName/$value`

#### Returns Raw Value in HTTP Body

```
<property-value>
```

## ASP.NET Core Implementation

### Add HasProperty() Helper Extension Methods

```csharp
public static class PropertyValueHelpers
{
    ...

    public static bool HasProperty(this object instance, string propertyName)
    {
        var propertyInfo = instance.GetType().GetProperty(propertyName);
       
        return (propertyInfo != null);
    }

    ...
}
```

### Add GetValue() Helper Method
```csharp
public static class PropertyValueHelpers
{
    ...

    public static object GetValue(this object instance, string propertyName)
    {
        var propertyInfo = instance.GetType().GetProperty(propertyName);
        if (propertyInfo == null)
        {
            throw new Exception($"Can't find property with name {propertyName}" );
        }
        return propertyInfo.GetValue(instance, new object[] {});
    }
    ...

}
```

### Add GetItemProperty Action Method

```csharp
using Microsoft.AspNetCore.Http.Extensions;
...

[HttpGet("Items({id})/Property1)")]
[HttpGet("Items({id})/Property2)")]
public async Task<IActionResult> GetItemProperty(Guid id)
{
    var item = await _repository
        .Items
        .FirstOrDefaultAsync(i => i.Id == id);
    
    if (item is null)
    {
        return NotFound();
    }

    var propertyToGet = new Uri(HttpContext.Request.GetEncodedUrl()).Segments.Last();

    if (!item.HasProperty(propertyToGet))
    {
        return NotFound();
    }

    var propertyValue = item.GetValue(propertyToGet);

    if (propertyValue == null)
    {
        return NoContent();
    }

    return Ok(propertyValue);
}

```

### Add GetItemProperty Action Method

```csharp
using Microsoft.AspNetCore.Http.Extensions;
...

[HttpGet("Items({id})/Property1)")]
[HttpGet("Items({id})/Property2)")]
public async Task<IActionResult> GetItemPropertyRawValue(Guid id)
{
    var item = await _repository
        .Items
        .FirstOrDefaultAsync(i => i.Id == id);
    
    if (item is null)
    {
        return NotFound();
    }

    var url = new Uri(HttpContext.Request.GetEncodedUrl());

    // ^2 index gets the second to last segment of the URI
    var prpertyToGet = new Uri(url).Segments[^2].TrimEnd('/');

    if (!item.HasProperty(propertyToGet))
    {
        return NotFound();
    }

    var propertyValue = item.GetValue(propertyToGet);

    if (propertyValue == null)
    {
        return NoContent();
    }

    return Ok(propertyValue.ToString());
}

```

### Add GetItemCollectionProperty Action Method

```csharp
...
[HttpGet("Items({id}/CollectionProperty1")]
[HttpGet("Items({id}/CollectionProperty2")]
...
[HttpGet("Items({id}/CollectionPropertyN")]
public async Task<IActionResult> GetItemCollectionProperty(Guid id)
{
   var collectionPropertyToGet = new Uri(HttpContext.Request.GetEncodedUrl()).Segments.Last();

   var item 
        = await _repository
            .Items
            .Include(collectionPropertyToGet)
            .FirstOrDefaultAsync(i => i.Id == id);
    
    if (item is null)
    {
        return NotFound();
    }

    if (!item.HasProperty(collectionPropertyToGet))
    {
        return NotFound();
    }

    return Ok(item.GetValue(collectionPropertyToGet));
}
```