# Updating an Entity

PATCH is the preferred methodolgy for updating entities.

PUT requires a complete update of the entity.

PATCH only requires the updated properties, reducing the data transmitted over the wire.

Upserting via PUT method is permissable according to the OData standard.

## Complete Update of Entity Through PUT Endpoint

```csharp
...
[HttpPut("Items({id})")]
public async Task<IActionResult> UpdateItem(Guid id, [FromBody] Item item)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    var itemInDatabase 
        = await _repository
                .Items
                .FirstOrDefaultAsync(i => i.Id == id);
    
    if (itemInDatabase is null)
    {
        return NotFound();
    }

    item.Id = id;  // ensure id on incoming JSON payload matches id in URI
    _repository.Update(item);
    _repository.SaveChangesAsync();

    return NoContent();
}
```
## Partial Update of an Entity Through PATCH Endpoint

Presently, there is a known issue with the `Delta<T>` class.  Attempting to update the Id of an object by including a different Id in the JSON payload is permitted, which is different from the OData standard, which states Ids provided in JSON payloads should be ignored. 

```csharp
using Microsoft.AspNetCore.OData.Deltas;
...

[HttpPatch("Items({id}")]
public async Task<IActionResult> PartiallyUpdatePerson(int id, [FromBody] Delta<Item> patch)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    var itemInDatabase 
        = await _repository
                .Items
                .FirstOrDefaultAsync(i => i.Id == id);
    
    if (itemInDatabase is null)
    {
        return NotFound();
    }

    patch.Patch(itemInDatabase);
    await _repository.SaveChangesAsync();
    
    return NoContent();
}
```
