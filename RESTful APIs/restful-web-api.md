# RESTful Web API Design

## Outer Facing Contract
* URI
* HTTP Method(s)
* Payload (optional)

## Naming Resources (URIs)
* No standard
* Best practices
  * URI should point to a noun
    * `api/authors`
    * `api/employees`
  * Noun should be followed by id
    * `api/authors/{authorId}`
  * Plural vs Singular
    * No standard
    * Be consistent to promote predictibility
  * Hierarchy
    * `api/authors/{authorId}/books`
    * `api/authors/{authorId}/books/{bookId}`
  * Filters
    * Not in URI
    * Provided in query string
    * `api/authors?orderBy=name`
  * Always exceptions
    * Remote procedure calls (RPCs)
    * `api/authors/{authorId}/totalAmountOfPages`
  * Ids
    * Auto-generated database Id
    * GUIDs
      * Allows switching database providers
      * Hides implementation details
      * Disadvantage -> readability (minimal)

## Interacting with Resources
| HTTP Method | Request Payload     | Sample URI        | Response Payload  | Use Case                            |
|-------------|---------------------|-------------------|-------------------|-------------------------------------|
| GET         | None                | /api/authors/{id} | single author     | Get entity                          |
| GET         | None                | /api/authors      | author collection | Get entity collection               |
| POST        | single author       | /api/authors      | single author     | Insert entity                       |
| PUT         | single author       | /api/authors/{id} | single author     | Update entire entity                |
| PATCH       | JSON Patch Document | /api/authors/{id} | single author     | Update selected entity fields       |
| DELETE      | None                | /api/authors/{id} | None              | Remove entity                       |
| HEAD        | None                | /api/authors/{id} | None              | Test if entity exists               |
| OPTIONS     | None                | /api/authors/{id} | None              | Get HTTP methods availble on entity |


## DTO vs Entity Model Objects
* DTO objects
  * Only used to return data to consumer
  * No need to validate inputs
* Tip: Use AutoMapper to map Entity model object fields to DTO object fields

## HTTP Status Code Levels
| Level | Meaning       |
|-------|---------------|
| 100   | Informational |
| 200   | Success       |
| 300   | Redirection   |
| 400   | Client Error  |
| 500   | Server Error  |

## HTTP Status Codes
| HTTP Status Code | Reason Phrase          |
|------------------|------------------------|
| 200              | OK                     |
| 201              | Created                |
| 204              | No Conent              |
|------------------|------------------------|
| 400              | Bad Request            |
| 401              | Unauthorized           |
| 403              | Forbidden              |
| 404              | Not Found              |
| 405              | Method Not Allowed     |
| 406              | Not Acceptable         |
| 409              | Conflict               |
| 415              | Unsupported Media Type |
| 422              | Unprocessable Entity   |
|------------------|------------------------|
| 500              | Internal Server Error  |

## Faults vs Errors
  * Error
    * Consumer passing incorrect data to API and it is correctly rejected
    * Level 400 status codes
  * Fault
    * API fails to provide response to valid request
    * Level 500 status codes

## Repository Pattern

### Benefits

Switch out persistence technology while still adhering to the contract defined by the repository interface(s).

Use different persistence technology depending upon requirements.  For example one query might use Entity Framework while another might use ADO.NET or even pure SQL.

## Getting Resources

### Working with Parent/Child Relationships
If a resource is always the child of a parent, the URI should reflect the mandatory relationship.

#### Implementation - Get All Children of Parent

Example URI: `/api/parent/{parentId}/children`

If parent does not exist, return Not Found status code

Get children for author from repository

Map children to DTOs

Return DTOs

#### Implementation - Get Child of Parent

Example URI: `/api/parent/{parentId}/children/{childId}`

If parent does not exist, return Not Found status code

If child does not exist, return Not Found status code

Map child to DTO

Return DTO

## Creating and Deleting Resources

## Method Safety
An HTTP method is safe if it does not change a resource.

## Method Idempotency
An HTTP method is idempotent when it has the same affect on the resource regardless the number of times it is executed.

| HTTP Method | Safe | Indempotent |
|-------------|------|-------------|
| GET         | Yes  | Yes         |
| OPTIONS     | Yes  | Yes         | 
| HEAD        | Yes  | Yes         |
| POST        | No   | No          |
| DELETE      | No   | Yes         |
| PUT         | No   | Yes         |
| PATCH       | No   | No          |

## Separate DTO Classes for response and request
  * Response <- `<Class>Dto`
  * Request -> `<Class>CreationDto`
  * Rationale
    * Different fields on each class
      * Age vs DateOfBirth
      * Name vs FirstName and LastName
    * Validation on CreationDto but not on Dto

## Creating a Resource with ASP.NET Core
  * Name route in method attribute
  * Accept `<class>CreationDto` object
  * If CreationDTO is null, return BadRequest()
  * Map CreationDTO to entity object
  * If repository save fails:
    * return StatusCode(500, message) or
    * throw new Exception(message) which is handled by global error handler
  * Map object to DTO
  * return CreatedAtRoute(routeName, routeValues, object)
    * CreatedAtRoute() is a helper method on Controller class
    * Adds Location header to HTTP response
    * Location header provides URI for created resource

## Creating a Child Resource with ASP.NET Core
  * URI: /api/parents/{parentId}/children
  * Example URI: /api/authors/{authorId}/books
  * Implmented in child's controller
  * Action method: `Create<Child>For<Parent>(parentId, childCreationDto)`
  * CreationDTO should not include parentId
  * Obtain parentId from route value
  * Method accepts parentId, creationDto
  * If CreationDTO is null, return BadRequest()
  * If parent != exist return NotFound()
  * Map to entity from creationDto
  * Save in repository
  * If save fails, throw new Exception
  * Map book entity to dto
  * return CreatedAtRoute(routeName, routeValues, object)

## Creating a Parent Resource with Multiple Children Resources
 * Add list of Children Resources to `<class>CreationDto`
 * Follow instructions in **Creating a Child Resource**

## Returning List of Specific Resources
* Create custom ModelBinder for list of GUIDs
  * Reads route value string that is a comma separated list of GUIDs
  * Returns list of GUIDs
* Method accepts list of ids
  * Provided in as route value
  * Example: Comma separated list of GUIDs
* If list of ids is null, return BadRequest()
* If ids.Count != entities.count, return NotFound()
* Map entities to DTO
* Return OK(List<Entities>)

## Creating a List of Resources
 * URI: /api/resourcescollection
 * Accept list of `<class>CreationDto`
 * Map entities to List<DTO>
 * routeValues = string that is a comma separated list of GUIDs
 * return CreatedAtRoute(routeName, routeValues, list<DTO>)

## POST to Exisiting Resource
  * POST to /api/resources/{id} where id exists
  * Return 409 - Conflict

## Accepting Additional Content-Types
  * Add XmlDataContractSerializerInputFormatter()
  * services.AddMvc(setup => setup.InputFormatters.Add(new XmlDataContractSerializerInputFormatter()))

## Deleting a Resource
  * If parent resource != exist, return NotFound()
  * If child resource != exist, return NotFound()
  * Delete book in repository
  * If delete fails, throw exception (handled by global exception handler)
  * Return status code 204 - No Content
  * ASP.NET Core Controller helper method NoContent()

## Deleting a Resource with Child Resources
  * If parent resource != exist, return NotFound()
  * Entity Framework Core
    * Cascade delete on by default
    * Deleting parent resource deletes child resources
  * Delete parent resource
  * If delete fails, throw exception
  * return NoContent()

## Deleting a Collection Resource
  * Rarely implemented because very destructive
  * Example: Deleting all authors deletes all books

## Updating Resources
  * Updating
    * PUT for full updates
    * PATCH for partial updates
  * Upserting
    * Creating a resource with PUT or PATCH

## Complete Update of Resource
  * Accept `<Class>ForUpdateDto` in HTTP payload
  * Dto should set fields to default values
  * If payload cannot be deserialized, return BadRequest()
  * If parent !exist return NotFound()
  * If child !exist
    * Map creationDto to entity object
    * Set id equal to id in URI routeValue
    * Add to repository
    * Throw exception if add fails
    * Map entity object to dto
    * return 201 Status Code
      * ASP.NET return CreatedAtRoute(routeName, routeValues, object)
  * Map Dto to entity
  * If update fails, throw exception
  * Return No Content or OK

## Updating a Collection of Resources
  * Rarely implemented because very destructive
  * Delete all existing resources
  * Replace with new collection

## Upserting
* HTTP PUT method can be used to create a resource
* If resource does not exist, PUT creates resource
* If resource exists, PUT updates resource

| Server Creates URI                    | Client Creates URI                  |
|---------------------------------------|-------------------------------------|
| PUT target existing URI               | PUT target nonexisting URI          |
| URI !exist return Not Found           | If resource !exist, create it       |
| POST must be used to create resources | PUT can be used to create resources |

## Partially Updating a Resource
* HTTP Patch is used for partial updates
* JSON Patch standard (RFC 6902)
* `Content-Type: application/json-patch+json`
  * Array of operations to be applied to resource
  * Example of JSON Patch:
    ```
    [
      {
        "op": "replace"
        "path": "/title"
        "value": "new title"
      },
      {
        "op": "remove",
        "path": "/description"
      }
    ]

## JSON Patch Operations
  * Add
  * Remove
  * Replace
  * Copy
  * Move
  * Test

## Partial Update of Resource
  * API Endpoint URI: `PATCH /parent/{parentId}/child/{childId}`
  * Accept `parentId`, `childId`, `JsonPatchDocument<childForUpdateDto>`
  * If validation fails, return Bad Request
  * If parent resource !exist, return Not Found
  * If child resource !exist
    * Create new ForUpdateDto
    * Apply patch to ForUpdateDto object
    * Map ForUpdateDto to entity object
    * Set entity object.id to childId from URI routeValues
    * Add book to repository
    * Throw exception if save fails
    * Return 201 Created response
      * ASP.NET return CreatedAtRoute(routeName, routeValues, object)
  * Map ForUpdateDto to entity object
  * Apply patch to entity object
  * Validate patch
  * Save entity
  * If save fails, throw exception
  * return No Content

## RESTful Validation
If HTTP request is syntactically correct, but semantically incorrect, a 400 level error should be returned.  The client made an error, not the server.  Returning a 500 level error (server error) is incorrect.

If HTTP request is syntactically correct but does not pass the custom logic of the system (e.g. the string length is greater than permitted) then status code 422 - Unprocessable Entity
should be returned.

The HTTP Response body should contain validation errors.

  * Do not report errors from database layer
  * Report validation at the DTO layer
  * OK to have two validation layers
    * Different validation rules at different layers
    * Example:
      * DTO class requires properties to be completed
      * Entity object does not require same properties

## RESTful Validation in ASP.NET Core

  * Apply attributes to properties on DTO objects
  * DataAnnotations namespace `System.ComponentModel.DataAnnotations`
  
  * Problem: ASP.NET does not provide helper method on Controller class that returns a result with status code 422 - Unprocessable Entity
  * Solution:
     * Create `UnprocessableEntityObjectResult` class
      * Inherits from `ObjectResult` class
      * Constructor
        * Overrides `ObjectResult` constructor
        * Accepts ModelStateDictionary
        * Pass `new SerializableError(modelStateDictionary)` to base
        * If `modelState` is null, throw argNullException
        * Sets StatusCode = 422

  * Validation for HTTP POST method
    * if arg representing resource is null
      * payload could not be parsed into required type
      * return Bad Request
    * if !ModelState.IsValid
      * return new UnprocessableEntityObjectResult
  
  * Validation for HTTP PUT method
    * Nearly identical to POST
    * Use abstract DTO classes to avoid duplicate validation attributes
    * Override properties to apply additional validation requirements

  * Validation for HTTP PATCH method
    * If JSON Patch semantics are invalid, return 422
    * Pass ModelState to JsonPatchDoc.ApplyTo()
    * ApplyTo() method adds errors to ModelStateDictionary
    * Validation occurs on JSON patch document, not entity
    * Must validate model after applying patch
    * Use Controller helper moethod TryValidateModel()
    * Upserting
      * If resource not found in repository (is null)
      * pass ModelState to JsonPatch.ApplyTo()
      * ApplyTo() method adds errors to ModelStateDictionary
      * TryValidateModel()
      * If not !Model.IsValid return UnprocessableEntityObjectResult

## Paging, Filtering and Searching
### Paging
Best practice to implement paging on all collection resources

Helps avoid performance issues
  * Limit the size of the result set returned from database
  * Limit the size of the result result transmitted to client

Paging parameters passed through query string of URI

Pagaing Parameters
* PageSize (capped at a maximum value)
* PageNumber (return first page by default)

Pagination Meta Data
  * Include in HTTP Response Custom Headers
  * Example custom header: `X-Pagination`
  * Example custom header value: JSON that includes pagination information
  * Links to previous and next pages
  * Total resource count
  * Total page count

### ASP.NET Core Paging

Create Parameters class to hold query parameters: `<Class>ResourceParameters`

Create `PagedList<TResource>` class to help create and hold pagination meta data.

`PagedList<TResource>` Class
* CurrentPage
* TotalPages
* PageSize
* TotalCount
* HasPrevious (calculated property)
* HasNext (calculated property)

`IRepository.Get<Class>()` method should accept `<Class>ResourceParameters` argument

`IRepository` should return a `PagedList<TResource>`

Use `IQueryable<T>` methods to implment paging
  * `IQueryable<T>.Skip()`
  * `IQueryable<T>.Take()`

Use `Url` property of `Controller` class to build URIs for previous and next pages of data.

### Filtering
  * Limits resources returned based on result of predicate
  * Pass fieldname as parameter in query string
  * Example: `/api/authors?genre=Fantasy`
  * Allow filtering on fields that are part of the resource
  * Do not allow filtering on a resource's:
    * Parent's field(s)
    * Children's field(s)
    * Entity object (resource is represented by DTO)
  
### Searching
  * API decides which fields to query against for the search term
  * Example: `/api/authors?searchQuery=King`

## Data Sorting and Shaping

### Sorting
Example URIs

Sort by one field
http://host/api/resource?orderBy=field

Sort by one field, descending
http://host/api/resource?orderBy=field descending

Sort on multiple fields
http://host/api/resource?orderBy=field1 descending, field2

### Sorting - ASP.NET Core Impmentation
Add `OrderBy` property to `<class>ResourceParameters` class  
Use ASP.NET Core model binding to bind `orderBy` query string parameter to `<class>ResourceParameters` class  
Pass `<class>ResourceParameters` object to repository class  
`EF<ResourceClass>Repository` class reads `orderBy` parameter  
Create `ApplySort()` extension method for `IQueryable<T>`  
Custom `IQueryable<T>.ApplySort()` method adds `OrderBy` statements to SQL query  

### Shaping Resources
Allows the consumer to choose the resource's fields that are included in the representation returned in the HTTP response.

Example

URI: `/api/parent?fields=id,name`

Return: `[ { id: 1, name: name1 }, {id: 2, name: name2 } ]`

### Shaping Resources in ASP.NET Core
Implement `IEnumerable<TSource>` extension method that accepts an `IEnumerable<TSource>` and a `string` that contains the comma separated list of fields that should be included and return an `IEnumerable<ExpandoOject>` with only the requested fields.

```csharp
public static IEnumerable<ExpandoObject> ShapeData<TSource>(this IEnumerable<TSource> source, string fields)
```

if `source` is `null` throw `ArgumentNullException`  
create `List<PropertyInfo>` using only one `TSource` object  
if `fields` is `null` or white space return all public fields  
else split comma separated list of field names into `string[]`  
trim field names  
add `PropertyInfo` to `List<PropertyInfo>`  
For each source object, create an `ExpandoObject`  
For each `PropertyInfo`:  
Get value of property from source object  
Add property and value to `ExpandoObject`  
Add `ExpandoObject` to `List<ExpandoObject>`  
return `List<ExpandoObject>`

## HATEOAS

## ASP.NET API Versioning
* Create custom
  * content-type
  * media-type
* Create custom Action Constraint Attribute
  * Descendent of Attribute class
  * Implements IActionConstraint
* Use Action Contrainst Attributes on Action Methods
  * Action Contrainst evaluates headers
    * media-type
    * content-type
  * Only enter Controller Action Method if matching media-type or content-type

## Rate Limiting

## Testing API

## Documenting API
* Swagger OpenAPI
* Swashbuckle

## HTTP `OPTIONS` Request
* Headers required in HTTP response to HTTP `OPTIONS` request
  * `Allow`
  * `Content-Length`
* `Allow` value is set to comma separated list of HTTP methods available on resource
* `Content-Length` is set to `0`
* Example:
  ```
  Allow: GET, OPTIONS, POST
  Content-Length: 0
  ```

## HTTP `HEAD` Request
* Identical to `GET` with one exception
* Exception: No response body
* Used by client to test:
  * Validity
  * Accessibility
  * Modification of resource