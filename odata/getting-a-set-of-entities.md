# Getting a Set of Entities

## Extend `ODataController`

`ODataController` extends `ControllerBase`

Applies `ODataRouting` attribute

```csharp
using Microsoft.AspNetCore.OData.Routing.Controllers;
...

[Route("/odata")]
public class MyClassController : ODataController
{
    private readonly IRepository _repository;

    public MyClassController(IRepository repository)
    {
        _repository = repository;
    }

    [HttpGet("Items")]
    public async Task<IActionResult> Get()
    {
        return Ok(await _repository.Items.ToListAsync());
    }
}
```
