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