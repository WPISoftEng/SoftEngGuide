# Starter Code

If you created your project using the starter code, then you have a little bit of the foundation for a JavaFX application, but there's still more to add to set you up for complete success.

## Understanding the starter code
Once you've imported the project into IntelliJ, you'll see that the code initially has two classes, `Main` and `App`. Both of these classes have main methods though. While you may be used to just having one 'main' java class, we use two here due to an issue with JavaFX running from a JAR file. Essentially, if the main class extends javafx.Application, then JavaFX needs to be bundled a certain way, and...it isn't. So, the workaround is to have a pseudo main class (`Main`) and the 'real' main class (`App`).

Let's look at these two classes

### Main.java

```java
package edu.wpi.teamname;

public class Main {

  public static void main(String[] args) {
    App.launch(App.class, args);
  }
}
```

This class is the entry point for the JVM. Its only purpose is to act as a buffer between the JVM and App. 

### App.java

```java
package edu.wpi.teamname;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

public class App extends Application {

  @Override
  public void init() {}

  @Override
  public void start(Stage primaryStage) {
    Label label = new Label("Text");
    label.setId("textLabel");
    label.setVisible(false);

    Button button = new Button("My Button");
    button.setId("showTextButton");
    button.setOnAction((e) -> label.setVisible(true));

    BorderPane root = new BorderPane();
    root.setRight(label);
    root.setLeft(button);

    Scene scene = new Scene(root, 200, 100);
    primaryStage.setScene(scene);
    primaryStage.show();
  }

  @Override
  public void stop() {}
}
```

This class is the starting point of the JavaFX side of our application. When `Main` calls `App.launch(...)`, the following life cycle occurs (from Oracle docs)

> The JavaFX runtime does the following, in order, whenever an application is launched:
  1. Constructs an instance of the specified Application class
  2. Calls the `init()` method
  3. Calls the `start(javafx.stage.Stage)` method
  4. Waits for the application to finish, which happens when either of the following occur:
    - the application calls `Platform.exit()`
    - the last window has been closed and the `implicitExit` attribute on Platform is true
  5. Calls the `stop()` method

`init` is empty by default, but it could be used to initialize other aspects of your application, such as your database or graph objects

If you run this app (using `./gradlew run` from the command line) you'll see a small window appear with no content. 

## Project Structure
As you will learn in lecture, a good design pattern for a GUI application is the Model-View-Controller pattern (also called Entitity-Controller-Boundary). This pattern contains three elements

- Models/Entities - These are your objects that have states. For a pathfinding application, the graph, along with its edges and nodes, would be part of the model. Any data classes would also be models. When the user interacts with your program, your models should change to update the state of the program. Using the Observer pattern, you can perform different actions as the state of the program changes.
- Views/Boundaries - Views are the FXML code that creates the GUI. A login view would create all of the elements that would be present on the login page. There is no user input handling in the view (although the FMXL will let you define a method in the controller that will handle user input).
- Controller - Controller classes handle the logic of interacting with the GUI. When you click on a button, a function in controller would be called. If a view needs to interact with a model, it must go through the controller.

## Scene vs Stage

When you're ready to begin adding content to your application, you'll need to add the content to a scene. By default, we have a stage created for us (`primaryStage`). But content must be added to a scene, and then the scene added to the stage. This means we have two options for changing the view
A) Change the content within the scene via `setRoot()`
B) Change the scene within the stage via `setStage()`
While it may seem trivial that you'd want to change the scene that is within the stage, this way will actually hurt our application once we enable full-screen due to an issue in JavaFX. To combat this, we must go with option A) Change the content within the scene via `setRoot()`

## Full-Screen Apps

I think everyone will agree that applications look best in fullscreen. You don't have a blank menu bar along the top, and you don't need to worry about the user trying to resize your window. Making your application fullscreen is easy. Simply call 

```java
primaryStage.setFullScreen(true)
```

Now the application will take up the entire screen, and appear on top of other apps by default. 
