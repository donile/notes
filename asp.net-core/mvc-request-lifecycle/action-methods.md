# Invoking Action Methods

Responsible for handling incoming requests

Provide the response data and response type

## Action Method Life Cycle

Managed by the `ControllerActionInvoker` class

Model Binding  
Action Filter (Before)
Action Method
Action Filter (After)

## `IActionResult`

Renders the response based upon the type

For example, the `IActionResult` renders the JSON that is returned in the HTTP response

## Action Filters

Implement the `IActionFilter` interface

Inherit from `Attribute`

Exposes two methods:
`OnActionExecuting(ActionExecutingContext context)`
`OnActionExecuted(ActionExecutedContext context)`

`OnActionExecuting()` runs before the Action Method

`OnActionExecuted()` runs after the Action Method

Can short-circuit the MVC life cycle by setting the `Result` property on the `context`

### Example Action Filter
```csharp
public class MyActionFilter : Attribute, IActionFilter
{
    public OnActionExecuting(ActionExecutingContext context)
    {
        // do stuff before the action method executes
    }

    public OnActionExecuted(ActionExecutedContext context)
    {
        // do stuff after the action method executed
    }
}