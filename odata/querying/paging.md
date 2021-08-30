# Paging Results

## Client Driven Paging

Use the `$top` and `$skip` query parameters

`http://my.domain.com/odata/Items?$top=10`

`http://my.domain.com/odata/Items?$top=10&$skip=2`


Configure the maximum amount of entities that may be requested with the `$top` query parameter for the entire application by calling the `SetMaxTop(N)` method in the `ConfigureServices()` method.

Configure the maximum amount of entities that may be requested with the `$top` query parameter for a specific action method by setting the `MaxTop` property on the `EnableQuery` attribute.

## Server Driven Paging

Enable the maximum amount of entities that are included in an action method's response by setting the `PageSize` property on the `EnableQuery` attribute of an action method.

Setting the `PageSize` attribute on the `EnableQuery` will include an `odata.nextLink` property on the JSON response.  This is the URI that the client can use to fetch the next entity in the result set.

## Get Total Number of Entities

### Use the `$count` query parameter

`http://my.domain.com/odata/Items?$count=true`

Includes an `@odata.count` metadata property on the JSON response.
