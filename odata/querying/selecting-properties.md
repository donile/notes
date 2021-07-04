# Selecting Properties

Return an entity with only a subset of it's properties.

## Use the `$select` query parameter

`http://my.domain.com/odata/Items?$select=propertyA,propertyB`

## Return an IQueryable

Applying the `EnableQuery` attribute to the action method causes the `IQueryable<T>` wrapped by and returned with the `ActionResult` to have a `.Select()` LINQ expression applied to it with the properties requested in the `$select` query parameter.  This means only the properties requested in the `$select` query parameter are requested from the database, reducing the size of the result sets.

There is a gotcha.  Do not call singleton queries or iterate over an `IQueryable<T>` because that will execute the query, resulting in multiple requests sent to the database.
