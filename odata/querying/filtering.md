# Filtering Results with OData Queries

## Filtering by Property Value

`http://my.domain.com/odata/Items?$filter=PropertyWithStringValue eq 'aString'`

`http://my.domain.com/odata/Items?$filter=PropertyWithStringValue ne 'aString'`

`http://my.domain.com/odata/Items?$filter=PropertyWithIntegerValue gt 0`

`http://my.domain.com/odata/Items?$filter=PropertyWithIntegerValue lt 1`

`http://my.domain.com/odata/Items?$filter=Date le 1981-03-07T00:00:00Z`


## Filtering by Enum Value

`http://my.domain.com/odata/Items?$filter=PropertyWithEnumValue eq Fully.Qualified.Enum.Type'EnumValue'`


## Filtering by More Than One Property Value

Use the `and` keyword

`http://my.domain.com/odata/Items?$filter=PropertyWithStringValue eq 'aString' and PropertyWithIntegerValue lt 1`


Use the `or` keyword

`http://my.domain.com/odata/Items?$filter=PropertyWithStringValue eq 'aString' or PropertyWithIntegerValue lt 1`



## Filtering by Collection Property Object's Values (OData Lambda Functions)

`http://my.domain.com/odata/Items?$filter=CollectionProperty/any(item:item/Property eq 'aString')`
