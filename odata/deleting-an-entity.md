# Deleting an Entity

```csharp
...
[HttpDelete("Items({id})")]
public async Task<IActionResult> DeleteItem(Guid id)
{
    var itemInDatabase 
        = await _repository
            .Items
            .FirstOrDefaultAsync(i => i.Id == id);
    
    if (item is null)
    {
        return NotFound();
    }

    _repository.Items.Remove(item);
    await _repository.SaveChangesAsync();

    return NoContent();
}
```
