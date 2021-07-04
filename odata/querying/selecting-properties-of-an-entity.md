## Selecting Properties of an Entity

The code below is not asynchronous and it results it two calls to the database, (1) when the `.Any()` LINQ expression is executed and (2) when the query is executed by the `EnableQuery` attribute after the action method completes.

While this results in two database calls, it saves on the size of the result sets transfered over the wire from the database to the application because only the requested properties are returned.

```csharp
using Microsoft.AspNetCore.OData.Query;
using Microsoft.AspNetCore.OData.Results;
...

[EnableQuery]
public IActionResult GetItem(Guid id)
{
    // itemsInDatabase is an IQueryable<T>
    var itemsInDatabase
        = _repository
            .Items
            .Where(i => i.Id == id);

    if (!itemsInDatabase.Any())
    {
        return NotFound();
    }

    return Ok(SingleResult.Create(itemsInDatabase));
}
```
