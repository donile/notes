# Action Results

Render the result of an action method to create the response object

Implement the `IActionResult` interface

Expose `ExecuteResultAsync(ActionContext context)`

Different implementations of `IActionResult` render different types of results (JSON, XML, HTML, etc.)

The Action Method selects the result type by executing a helper method.

## Result Type Helper Methods
Content()
Json()
View()

## Result Types
ContentResult -> string  
JsonResult -> JSON  
ViewResult -> Razor page  
FileResult -> *.doc file  

## Result Filters

Inherit from `Attribute`

Implment `IResultFilter` interface

Runs before and after `IActionResult.ExecuteResultAsync()`

Exposes:

`OnResultExecuting(ResultExecutingContext context)`

`OnResultExecuted(ResultExecutingContext context)`