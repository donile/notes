# Incomplete Implementations

## Partially Implemented

`$it` operator cannot refer to outter context yet

## Missing Implementations

### `$root` literal is not supported yet

#### Example (does not presently work)

`http://my.domain.com/odata/People?$filter=FirstName eq $root/People(1)/FirstName`

### `$search` query option is not supported yet
