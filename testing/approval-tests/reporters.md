# ApprovalTests: Reporters

## Reporters
Only executed on failure

Launch application to review received and approved files

Allow user to approve result

## Assigning a Reporter
Apply the `UseReporter` attribute to the test method to specify the application to use to compare the received and approved files.

The `FileLauncherReporter` will open the received and approved files with the program assigned to open files for a given file extension.
```csharp
[Test]
[UseReporter(typeof(FileLauncherReporter))]
public void TestMethod()
{
    ...
    Approvals.Verify(object); // creates a .txt file
    Approvals.VerifyHtml(html); // creates a .html file
}
```

## Multiple Reporters
Open more than one reporter application for comparing received and approved files by specifying more than one reporter in the `UseReporter` attribute constructor.
```csharp
[Test]
[UseReporter(typeof(FileLauncherReporter), typeof(DiffReporter))]
public void TestMethod()
{
    ...
    Approvals.Verify(object);
}
```