## Selecting Properties of an Entity Set

```csharp
[EnableQuery]
public IActionResult GetItems()
{
    // Items returns an IQueryable<T>
    return Ok(_repository.Items);
}
```
