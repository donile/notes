# Containment Navigation

Prevent child navigation properties, that only exist within the context of the parent entity and not by themselves, from being accessed outside of the context of the parent entity.

## Example

### Prevent

`http://my.domain.com/odata/Child(1)`

### Permit

`http://my.domain.com/odata/Parent(1)/Child`

## Define Containment

Apply `Contained` attribute to the navigation property on the POCO model class.

