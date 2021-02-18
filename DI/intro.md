# Dependency Injection

In large applications, such as the hospital kiosk, some parts of the application will depend on other parts, such as the service request view depending on getting updates from the database, or the navigation view depending on the graph, pathfinding, and possibly database as well. 

To help manage these dependencies, we can use a Dependency Injector. The injector will handle all the necessary logic of providing the dependencies to the parts of the application that need it. All you do is define the dependencies needed, and the injector handles any object creation that may need to take place. No more `new`. 

Take a look at the [Getting Started](https://github.com/google/guice/wiki/GettingStarted) guide for Google Guice to get a better understanding of DI, with examples. Then, read through the wiki, notable, Scopes, Bindings, Linked Bindings, Instance Bindings, @Provides Methods, and Provider Bindings. 

Be sure to read through all the best practices too!

And for those who think that dependency injection is too advanced for this course, I ask you "how do your fxml objects get into the controller classes?"
