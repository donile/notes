# Creating a Resource

Presently, the `ApiController` attributes is not compatiable with the `ODataController` base class.

Additionally, without the `ApiController` attribute applied to the class, complex types are not automatically deserialized, so the `FromBody` attribute must be applied to the action method's parameter that will contain the incoming JSON payload after it has been deserialized.

Lastly, since the `ApiController` attribute cannot be applied, a `BadRequestObject` is not automatically returned when the ModelState is invalid; therefore it must be checked and handled within the action method.

## Create Resource Endpoint
```csharp
...
[HttpPost("Items")]
public async Task<IActionResult> CreateItem([FromBody] Item item)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    _repository.Items.Add(item);
    await _repository.SaveChangesAsync();

    return Created(item);
}
```
