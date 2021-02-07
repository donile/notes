# Entity Framework Core Testing

## Definitions

### Unit Tests
Test small, individual components (classes)  
Does not integrate external resources (filesystem, database, etc.)  
Mock away dependencies  
Extremely fast  


### Integration Tests
Test how two more more components (classes) work together  
Determine if changes in environment result in failure  
Often use the same types of frameworks as unit tests  
Test failure does not usually define which component failed  

### Functional Tests
Tests full request/response cycle as a user  
Can be manual or automated  
Tools: Selenium
High complexity, slow execution, poorly encapsulated

## DbContext Constructor
Add a public constructor that accepts a `DbContextOptions<TDbContext>` parameter to make it possible to switch the type of database used by the `TDbContext`.

```csharp
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
    {
        // ...
    }
}
```

## Unit Test Example
```csharp
[Test]
public void Given_DoSomething_Then()
{
    var options = new DbContextOptionsBuilder<AppDbContext>()
        .UseInMemoryDatabase("InMemoryDatabaseName")
        .Options;

    using (var dbContext = new AppDbContext(options))
    {
        // add data to database
        
        dbContext.SaveChanges();

        var repository = new Repository(dbContext);

        // act
        var result = repository.DoSomething();

        // assert
        Assert.That(result, Has.Exactly(1).Items);
    }
}
```

## Unique Database Per Test
```csharp
[Test]
public void Given_DoSomething_Then()
{
    var options = new DbContextOptionsBuilder<AppDbContext>()
        .UseInMemoryDatabase($"InMemoryDatabaseWith.{Guid.NewGuid()}")
        .Options;

    // ...
}
```

## Unique DbContext Per Operation
In an ASP.NET Core application, each HTTP request receives a new `DbContext`.  Using a new `DbContext` for each operation in a unit test helps to mimic that behavior.
```csharp
[Test]
public void Given_DoSomething_Then()
{
    var options = new DbContextOptionsBuilder<AppDbContext>()
        .UseInMemoryDatabase($"InMemoryDatabaseWith.{Guid.NewGuid()}")
        .Options;

    // arrange
    using (var dbContext = new DbContext(options))
    {
        // setup database for test
        // ...
    }
    
    // act
    IList<Item> result;
    using (var dbContext = new DbContext(options))
    {
        var repository = new Repository(dbContext);
        
        result = repository.GetItems();
    }

    // assert
    Assert.That(result, Has.Exactly(1).Items);
}
```

## In Memory Database Advantages and Disadvantages

### Advantages
Fast  
Lightweight  
Replacement for mocking `DbContext` (which is difficult)

## Disadvantages
Not a relational database
No referential integrity checks
No DefaultValueString(string)
No TimeStamp
No IsRowVersion

## Use SQLite
```csharp
[Test]
public void Given_DoSomething_Then()
{
    var connectionStringBuilder = new SqliteConnectionStringBuilder
    {
        Datasource = ":memory"
    };

    var connection = new SqliteConnection(connectionStringBuilder.ToString());
    
    var options = new DbContextOptionsBuilder<AppDbContext>()
        .UseSqlite(connection)
        .Options;
    
    using (var dbContext = new DbContext(options))
    {
        dbContext.Database.OpenConnection();
        dbContext.EnsureCreated();
    }

    // ...
}
```

## SQLite Limitations
Schemas  
SQL Sequences  
Computed columns  
User-defined functions  
Fragment default values  
Column Types  