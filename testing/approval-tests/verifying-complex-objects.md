# ApprovalTests: Verifying Complex Objects

Override the object's `ToString()` method to return meaningful output to be written to the received file.

```csharp
public class SystemUnderTest()
{
    ...
    ToString()
    {
        StringBuilder sb;
        ...
        // build string representation of object that will be written to received file constructed by Approvals.Verify() method
        ...
        return sb.ToString();
    }
    ...
}
```

By default the `Approvals.Verify(object)` method calls the object's `ToString()` method to write to the received file which is then compared to the approved file.

```csharp
[Test]
[UseReporter(typeof(DiffReporter))]
public void VerifyStateOfSystemUnderTest()
{
    var sut = new SystemUnderTest();

    sut.DoSomething(aVariable);

    Approvals.Verify(sut);
}
```