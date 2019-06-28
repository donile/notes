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