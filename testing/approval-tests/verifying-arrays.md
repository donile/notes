# ApprovalTests: Verifying Arrays


Verifying the items in an `IEnumerable<T>` with the `VerifyAll()` method
```csharp
[Test]
[UseReporter(typeof(DiffReporter))]
public void VerifyItemsInIEnumerable()
{
    var collection = new List<string>();
    ...
    // act on collection
    ...
    Approvals.VerifyAll(collection, "label"); // with label
    Approvals.VerifyAll(collection, "label", x => $"added-format {x}"); // with label and format
}
```

Received file contents With label
```
label[0] = item 0
label[1] = item 1
```

Received file contents With label and format
```
label[0] = added-format item 0
label[1] = added-format item 1
```