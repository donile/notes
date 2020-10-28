# ApprovalTests: Scrubbing Out Text

Replace non-deterministic text in the received file produced during test.  Use this methodology with caution because if there is an error in the way the scrubbed data is formatted the test will pass even though the formatting is incorrect.
```csharp
[Test]
[UseReporter(typeof(DiffReporter))]
public void VerifyNonDeterministicTextIsCorrect()
{
    var text = $"Report Date: {DateTime.Now}";

    Approvals.Verify(
        text,
        (receivedFileText) =>
        {
            Regex.Replace(
                receivedFileText, 
                "Report Date.*", 
                "Report Date: January 1, 2020"
            );
        }
    );
}
```