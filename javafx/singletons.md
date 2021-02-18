# Singletons

Before I say anything else, I want to clarify that singletons are **not always bad**.
Many people will say that singletons are bad practice, but their solutions to the same problems are almost never truly without singletons or singleton-like behavior.
For example, Guice allows us to inject a single instance of an object to each class that depends on it, without relying on the standard `getInstance` method.

## Why do we need them?
Singletons allow an application to rely upon one single source of truth.
The database that holds the nodes and edges is an example of a kind of singleton.
There is only one database that you rely on for the data.
Similarly, when we discuss the state of the program, we need to be sure there aren't conflicting values.

The FXML controllers will use these single sources of truth to communicate with each other.
One view controller might set the value of the currently selected node, while another view controller displays information about the currently selected node.
To ensure they are both referencing the same node object, we need a single instance of the state class that represents the selected node to be accessible to both of them. 
Guice helps us do this.

## Creating a singleton with Guice

Creating a singleton is as simple as creating a new injection module.

```java
public class StateProvider extends AbstractModule {

  @Provides
  @Singleton
  public ViewState provideViewState() {
    return new ViewState();
  }
}
```

This module creates a single `ViewState` instance, and will inject that same instance to every class that depends on it.
Combining this with JavaFX's properties and bindings allows for a super simple way of *properly* implementing ECB/MVC in your applications.