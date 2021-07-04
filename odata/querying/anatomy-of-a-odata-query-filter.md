# Anatomy of an OData Query Filter

An OData Query Filter is an Action Filter that has a three step process.

## Three Step Process

Parsing

Validation

Application

### Parsing

Inspect the URI and parse the revelant parts of the query string

### Validation

Check if what was requested is allowed.

### Application

Convert the query options from the OData query into a LINQ expression and process it.

## Present Issues

Combining the `EnableQuery` attribute with async code is presently not supported.
 