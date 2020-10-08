# NUnit Assertions

## Asserting on Equality

`Is.EqualTo()` executes the type's `Equals()` method.

To compare two reference types for equality based upon a condition other than the equality of their memory addresses, override the Equals() method with a custom implementation.  For example, compare the `Id` property of each type.

Verify `a.Equals(b)` returns true.

```csharp
Assert.That(a, Is.EqualTo(b));
```

## Asserting on Reference Equality (Memory Address Equality)

Verify `a` points to the same object in memory as `b`.

```csharp
Assert.That(a, Is.SameAs(b));
```

## Asserting on Floating Point Values

Verify `a` is equal to `b` plus or minus 0.004.

```csharp
Assert.That(a, Is.EqualTo(b).Within(0.004));
```

Verify `a` is greater than or equal to 90% of `b` and `a` is less than or equal to 110% of `b`.
```csharp
Assert.That(a, Is.EqualTo(b).Within(10).Percent);
```

## Asserting on Collections

Verify collection has specific number of items.
```csharp
Assert.That(collection, Has.Exactly(1).Items);
```

Verify all items int the collection are different.
```csharp
Assert.That(collection, Is.Unique);
```

Verify a specific item exists in the collection.
```csharp
Assert.That(collection, Does.Contain(item));
```

Verify an item that returns a set of values exists in the collection.
```csharp
Assert.That(collection, Has.Exactly(1)
                            .Property("PropertyName1").EqualTo("PropertyValue")
                            .And
                            .Property("PropertyName2").EqualTo(1)
                            .And
                            .Property("PropertyName3").EqualTo(2));
```

Verify an item that returns a set of values exists in the collection, in a type safe methodology.
```csharp
Assert.That(collection, Has.Exactly(1)
                            .Matches<ItemClass>(
                                item => item.PropertyName1 == "PropertyValue" && 
                                        item.PropertyName2 == 1 &&
                                        item.PropertyName2 == 2
                            ));
```

## Asserting Exceptions are Thrown

Verify an exception of `ExceptionType` was thrown.
```csharp
Assert.That(() => _sut.Method(), Throws.TypeOf<ExceptionType>());
```

Verify an exception of `ExceptionType` was thrown and it contains certain property values.
```csharp
Assert.That(() => _sut.Method(), Throws.TypeOf<ExceptionType>()
                                        .With
                                        .Property("Message")
                                        .EqualTo("The Message"));
```

Verify an exception of `ExceptionType` was thrown and it contains certain property values, using a type safe methodology.
```csharp
Assert.That(() => _sut.Method(), Throws.TypeOf<ExceptionType>()
                                        .With
                                        .Matches<ExceptionType>(
                                            ex => ex.Message == "The Message"
                                        ));
```