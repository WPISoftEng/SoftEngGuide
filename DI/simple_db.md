# Simple Google Guice Dependency Injection Example

Let's look at a simple example for using dependency injection to access the database from a javafx controller. 


## Database Setup

Begin by creating the database class

```java
public class DatabaseService {

}
```

The database class requires a `Connection` in order to connect to the database. This `Connection` will be injected by guice so that during testing we can change how the database exists (in memory vs not).

```java
public class DatabaseService {

	private Connection connection;
	
	// @Inject tell Guice that this variable needs to be injected,
	// rather than having the user instantiate.
	@Inject
	public DatabaseService(Connection connection) {
		this.connection = connection;
	}
}
```

Notice how we don't have the details of the connection this database is going to make. That's a good thing. The database class shouldn't worry about the lower level connection to the databse. It should only be concerned about reading and writing to the database through this 'mysterious' connection. 

Let's create two methods: one to initialize a table in the database, and one to read from it. 


```java
public class DatabaseService {
	private Connection connection;
	
	// @Inject tell Guice that the variables needs to be injected,
	// rather than having the user instantiate.
	@Inject
	public DatabaseService(Connection connection) {
		this.connection = connection;
	}
		
	public void initTable() {
		try {
			String query =
				"CREATE TABLE Employees( "
					+ "Id INT NOT NULL GENERATED ALWAYS AS IDENTITY, "
					+ "Name VARCHAR(255), "
					+ "Salary INT NOT NULL, "
					+ "Location VARCHAR(255), "
					+ "PRIMARY KEY (Id))";
			
			Statement stmt = connection.createStatement();
			stmt.execute(query);
			
			String insertQuery = "INSERT into Employees(Name, salary, location) VALUES('Wilson Wong', 100000, 'WPI')";
			stmt.execute(insertQuery);
		} catch (SQLException e) {
			log.error(e.getMessage());
		}
	}
	
	public String getEmployeeName() {
	    try {
	      Statement stmt = connection.createStatement();
	      ResultSet res = stmt.executeQuery("SELECT Name from Employees");
	      if (res.next()) {
	        return res.getString("Name");
	      }
	    } catch (SQLException throwables) {
	      log.error(throwables.getMessage());
	    }
	    return null;
	}
}
```

Here we have created two mehtods, one that sets up a table for employees, and one that reads the name from it.

## Creating the Injectable Connection

Our `DatabaseService` relies on a `Connection` in order to work. By default, Guice will inject a no-arg constructor instance of `Connection`, but we need to create a specific `Connection` in order for our database to work.

```java
/**
 * This is a Guice Module config class. This module forces guice to provide an instance of the
 * Connection as a singleton.
 */
public class ConnectionProvider extends AbstractModule {
	
	private final String realDbUrl = "jdbc:derby:myDB;create=true";
	private final String memoryDbUrl = "jdbc:derby:memory:myDB;create=true";

	
	@Override
	protected void configure() {
	bind(Connection.class)
		.annotatedWith(Names.named("RealDB"))
		.toInstance(DriverManager.getConnection(realDbUrl));
	bind(Connection.class)
		.annotatedWith(Names.named("MemoryDB"))
		.toInstance(DriverManager.getConnection(memoryDbUrl));
	}
}
```

Most of this code should look familiar from the Guice starter guide. The important lines of code are the bindings in `configure()`. The bindings use `Named` bindings to inject either a real database connection or a connection to an in memory database. We can update our `DatabaseService` code to use the in-memory database for this example because it will be reset each time the program is run. 

By binding `Connection` to a specific instance, we don't need to worry about multiple connections being spawned. Normally you would need to be careful about this, since only one connection is allowed at a time, but using Guice to inject the connection where needed means the programmer cannot mess up.

```java
@Inject
public DatabaseService(@Named("MemoryDB") Connection connection) {
	this.connection = connection;
}
```


## Registering `ConnectionProvider` with Guice

The final step is to register the `ConnectionProvider` class with Guice so that it can inject using our configuration. 

In `App.java`:

```java
@Override
public void init() {
	log.info("Starting Up");
	Injector injector = Guice.createInjector(new ConnectionProvider());
	    
	DatabaseService db = injector.getInstance(DatabaseService.class);
	db.initTable();
}
```

Here we access the database through the guice inector directly, but if you were to use this in a **controller** class, you would simply inject it the same way we injected the `Connection`. 

```java
public class ViewController {

	private DatabaseService db;
	
	@Inject
	public ViewController(DatabaseService db) {
		this.db = db;
	}
	
	...
}
```

Now we have easy access to a database that *hides the implementation* from the user. If the database changes constructor arguments, you don't need to locate where you create the database because you *don't* create the database manually.