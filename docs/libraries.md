# Built In Libraries

## Shared

### Framas.DbConnectionProvider

The **Framas.DbConnectionProvider** implements the `IDbConnectionContainer` interface that provide functions to get the `DbConnection` object
based on specified **Name** and the current **AppId** and **mesocomp** 

The `IDbConnectionContainer` will find the db connection info that is is store in table `FT017` (class `Framas.DbConnectionProvider.FT017_DbConnectionInfo`) to create the `DbConnection` object

#### How to Manage FT017_DbConnectionInfo

<!-- tabs:start -->

#### **Visual Studio**

> FramasVSIX is required. [Click here to learn how to install](/README?id=install-framasvsix).

Open Visual Studio and following below steps:

1. Open Visual Studio. Goto Extensions -> Framas -> ManageApplications
2. Select which location you want to modify DbConnection.
3. Click **DB Connections** on ribbon menu to open the form.
4. Create/Update/Delete db connection you want to.

![](/assets/gif/vs_manage_dbconnections.gif)

<!-- tabs:end -->

#### Properties of DBConnection
- **Name**: The name of the db connection
- **DbProvider**: The database provider of the connection
    * mssql: Microsoft SQL Server
    * sqlite: SQLite
    * mysql: MySQL
    * npgsql: PostgreeSQL
- **ConnectionString**: The connection string
- **mesocomp**: The company identifier.
- **AppId**: The application identifier that db connection belong to.
- **Note**: Some note about the connection

#### How to Install

Add below packages into the project

```
<PackageReference Include="Framas.DbConnectionProvider" Version="8.0.0" />
```

#### How to Use

<!-- tabs:start -->

#### **AspNetCore**

```cs
using Dapper;
using Framas.DbConnectionProvider;
using Microsoft.AspNetCore.Mvc;

namespace ExampleControllers;

public class ExampleController1 : ControllerBase
{
    private readonly IDbConnectionContainer _dbConnContainer;

    public ExampleController1(IDbConnectionContainer dbConnContainer, IServiceProvider services)
    {
        // Get the IDbConnectionContainer service by dependency injection
        _dbConnContainer = dbConnContainer;

        // Or you can get by IServiceProvider
        _dbConnContainer = services.GetRequiredService<IDbConnectionContainer>();
    }

    [HttpGet]
    public ActionResult GetData()
    {
        // Create the DbConnection object by the connection name 'Connection1'
        DbConnection conn = dbConnContainer.CreateDbConnection("Connection1");

        // Execute the query
        var result = conn.Query("SELECT * FROM table");
        return Ok(result);
    }
}

```

#### **Winforms**

```cs
using Dapper;
using Framas.BootLoader.Winforms;
using Framas.DbConnectionProvider;
using System.Data.Common;

public void Execute()
{
    // Get the IDbConnectionContainer service
    var dbConnContainer = FramasWinformsApp.GetRequiredService<IDbConnectionContainer>();

    // Create the DbConnection object by the connection name 'Connection1'
    DbConnection conn = dbConnContainer.CreateDbConnection("Connection1");

    // Execute the query
    var result = conn.Query("SELECT * FROM table");
}
```

<!-- tabs:end  -->

#### IDbConnectionContainer

```cs
using System.Data.Common;
using System.Threading;
using System.Threading.Tasks;

namespace Framas.DbConnectionProvider;

/// <summary>
/// Interface for a container that manages database connections.
/// </summary>
public interface IDbConnectionContainer
{
    /// <summary>
    /// Creates a database connection with the specified name.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <returns>A <see cref="DbConnection"/> object.</returns>
    DbConnection CreateDbConnection(string name);

    /// <summary>
    /// Creates a database connection with the specified name and MESOCOMP value.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <param name="mesocomp">The MESOCOMP value.</param>
    /// <returns>A <see cref="DbConnection"/> object.</returns>
    DbConnection CreateDbConnection(string name, string mesocomp);

    /// <summary>
    /// Gets the connection information for the specified name.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <returns>An <see cref="IDbConnectionInfo"/> object.</returns>
    IDbConnectionInfo GetConnectionInfo(string name);

    /// <summary>
    /// Gets the connection information for the specified name and MESOCOMP value.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <param name="mesocomp">The MESOCOMP value.</param>
    /// <returns>An <see cref="IDbConnectionInfo"/> object.</returns>
    IDbConnectionInfo GetConnectionInfo(string name, string mesocomp);

    /// <summary>
    /// Asynchronously creates a database connection with the specified name.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <param name="cancellationToken">A cancellation token.</param>
    /// <returns>A task that represents the asynchronous operation. The task result contains a <see cref="DbConnection"/> object.</returns>
    Task<DbConnection> CreateDbConnectionAsync(string name, CancellationToken cancellationToken = default);

    /// <summary>
    /// Asynchronously creates a database connection with the specified name and MESOCOMP value.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <param name="mesocomp">The MESOCOMP value.</param>
    /// <param name="cancellationToken">A cancellation token.</param>
    /// <returns>A task that represents the asynchronous operation. The task result contains a <see cref="DbConnection"/> object.</returns>
    Task<DbConnection> CreateDbConnectionAsync(string name, string mesocomp, CancellationToken cancellationToken = default);

    /// <summary>
    /// Asynchronously gets the connection information for the specified name.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <param name="cancellationToken">A cancellation token.</param>
    /// <returns>A task that represents the asynchronous operation. The task result contains an <see cref="IDbConnectionInfo"/> object.</returns>
    Task<IDbConnectionInfo> GetConnectionInfoAsync(string name, CancellationToken cancellationToken);

    /// <summary>
    /// Asynchronously gets the connection information for the specified name and MESOCOMP value.
    /// </summary>
    /// <param name="name">The name of the connection.</param>
    /// <param name="mesocomp">The MESOCOMP value.</param>
    /// <param name="cancellationToken">A cancellation token.</param>
    /// <returns>A task that represents the asynchronous operation. The task result contains an <see cref="IDbConnectionInfo"/> object.</returns>
    Task<IDbConnectionInfo> GetConnectionInfoAsync(string name, string mesocomp, CancellationToken cancellationToken);
}
```



## Winforms

### Security

---

## AspNetCore

### WORK IN PROGESS
