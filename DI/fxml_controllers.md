# Using Dependency Injection with FXML Controllers

FXML controllers are automatically instantiated and linked to their `.fxml` file when they are loaded. We can customize the behavior however to provide our own custom controller at runtime. This is necessary to inject dependencies through the constructor. 

## Overriding the **Controller Factory**

When an `fxml` file is linked to a controller, the `FXMLLoader` handles creating the instance of the controller class. We can change how this happens using `FXMLLoader::setControllerFactory`. This method allows us to provide the loader with a method that will return the correct fxml controller class. We will use `Injector::getInstance` as the factory.[^1]

[^1]: The factory design pattern is used to handle generating instances of other classes.

Using the DatabaseService from [Simple Database example](simple_db.md)

In `App.java`

```java
public class App extends Application {

  private FXMLLoader loader;

  @Override
  public void init() {
    log.info("Starting Up");
    Injector injector = Guice.createInjector(new DatabaseServiceProvider());
    
    loader = new FXMLLoader();
    loader.setControllerFactory(injector::getInstance);
  }

  @Override
  public void start(Stage primaryStage) throws IOException {
  	 // The controller used in HomeView.fxml will be created by the Guice Injector
    Parent root = loader.load(getClass().getResourceAsStream("views/HomeView.fxml"));
  }
  ...
}
```

In `HomeViewController.java`

```java
public class HomeViewController {

  private DatabaseService db;
  
  @Inject
  public HomeViewController(DatabaseService db) {
    this.db = db;
  }
}
```

## Creating more `FXMLLoader`s

Due to how FXMLLoader is implemented, you can only load one FXML file from it before you need a new one. Therefore, we need to be able to create more FXMLLoaders every time we want to change the scene. To do this, we just create a Guice module that sets up the FXMLLoader for us.

```java
package edu.wpi.teamname.views;

import com.google.inject.AbstractModule;
import com.google.inject.Injector;
import com.google.inject.Provides;
import javafx.fxml.FXMLLoader;

public class FXMLLoaderProvider extends AbstractModule {

  @Provides
  public FXMLLoader provideFXMLLoader(Injector injector) {
    FXMLLoader loader = new FXMLLoader();
    loader.setControllerFactory(injector::getInstance);
    return loader;
  }
}
```

Guice will always inject the `Injector` if your `@Provides` method depends on it, so we leverage this to create a new `FXMLLoader` with the correct controller factory set. Now in our `HomeController` we can use this preconfigured `FXMLLoader` to change views.

```java
public class HomeViewController {

  private DatabaseService db;
  private FXMLLoader loader;
  
  @Inject
  public HomeViewController(DatabaseService db, FXMLLoader loader) {
    this.db = db;
    this.loader = loader;
  }
  
  ...
  
  private void switchScene() {
    loader.load(getClass().getResourceAsStream("...")); // Generates controller from Guice
    ...
  }
}
```
