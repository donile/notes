# Data Driven Tests

## Specify Input and Expected Values as Method Attributes

The values passed to the the `TestCase` attribute are passed as the arguments to `TestMethod()`.

```csharp
[Test]
[TestCase(1, 1, 2)]
[TestCase(1, 2, 3)]
public void TestMethod(int input1, int input2, int expected)
{
    int actual = input1 + input2;
    Assert.That(actual, Is.EqualTo(expected));
}
```

## Assert on Value Returned by Test

Set the `ExpectedResult` property of the `TestCase` attribute and return the actual computed value and NUnit will perform the comparison.

```csharp
[Test]
[TestCase(1, 1, ExpectedResult = 2)]
[TestCase(1, 2, ExpectedResult = 3)]
public int TestMethod(int input1, int input2)
{
    return input1 + input2;
}
```

## Centralized Test Data (Explicit Assert in Test Method)

Declare a class that has a member that returns an IEnumerable.
```csharp
public class TypeContainingTestData
{
    public static IEnumerable TestCases
    {
        get
        {
            yield return new TestCaseData(1, 1, 2);
            yield return new TestCaseData(1, 2, 3);
        }
    }
}
```

Use the `TestCaseSource` attribute to instruct NUnit which class and member should be used to access the test data.
```csharp
[Test]
[TestCaseSource(typeof(TypeContainingTestData), "TestCases")]
public void TestMethod(int input1, int input2, int expected)
{
    int actual = input1 + input2;
    Assert.That(actual, Is.EqualTo(expected));
}
```

## Centralized Test Data (Assert on Value Returned by Test)

Declare a class that has a member that returns an IEnumerable.
```csharp
public class TypeContainingTestData
{
    public static IEnumerable TestCases
    {
        get
        {
            yield return new TestCaseData(1, 1).Returns(2);
            yield return new TestCaseData(1, 2).Returns(3);
        }
    }
}
```

Use the `TestCaseSource` attribute to instruct NUnit which class and member should be used to access the test data.

```csharp
[Test]
[TestCaseSource(typeof(TypeContainingTestData), "TestCases")]
public int TestMethod(int input1, int input2)
{
    return input1 + input2;
}
```

## Reading Test Data from an External File

Create a file (TestData.csv) contaiing the test data.

```csv
1, 1, 2
1, 2, 3
```

Create a class with a public member that accepts the name of the file with test data as an argument and returns an IEnumerable of TestCase objects.

```csharp
public class TestCaseReader
{
    public static IEnumerable GetTestCasesFromFile(string filename)
    {
        // read test data from a file into IEnumerable<TestCaseData>
        // return IEnumerable<TestCaseData>
    }
}
```

```csharp
[Test]
[TestCaseSource(typeof(TestCaseReader), "GetTestCasesFromFile", new object[] { "TestData.csv" })]
public int TestMethod(int input1, int input2)
{
    return input1 + input2;
}
```