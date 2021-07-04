## Select Properties of a Navigation Property

```csharp
[HttpGet("Items({id})/Property")]
[EnableQuery]
public IActionResult GetPropertyForItem(int id)
{
    var item = _repository.Items.FirstOrDefault(i => i.Id == id);

    if (item is null)
    {
        return NotFound();
    }

    return Ok(_repository.Property.Where(p => p.Item.Id == id));
}
```
