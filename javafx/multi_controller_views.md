# Multi-Controller Views

Some views in your application can get rather large. The main view might have the pathfinding, login, service requests, and more.

In order to make the controllers for these views smaller, which means both easier to maintain and easier to test, you can combine fxml views together, while each maintaining its own controller.

We do this using the `fx:include` tag.


<!-- Taken from https://riptutorial.com/javafx/example/7285/nested-controllers -->


## Example

This is a fxml containing a StackPane with a Text node. The controller for this fxml file allows getting the current counter value as well as incrementing the counter:

`counter.fxml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<?import javafx.scene.text.*?>
<?import javafx.scene.layout.*?>

<StackPane prefHeight="200" prefWidth="200" xmlns:fx="http://javafx.com/fxml/1" fx:controller="counter.CounterController">
    <children>
        <Text fx:id="counter" />
    </children>
</StackPane>
```

`CounterController.java`

```java
package counter;

import javafx.fxml.FXML;
import javafx.scene.text.Text;

public class CounterController {
    @FXML
    private Text counter;

    // CountState entity object
    private CountState countState;

    public void initialize() {
        counter.setText(Integer.toString(countState.getValue()));
    }
}
```

### Including FXML

`Outer.fxml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>

<BorderPane prefHeight="500" prefWidth="500" xmlns="http://javafx.com/javafx/8" xmlns:fx="http://javafx.com/fxml/1" fx:controller="counter.OuterController">
    <left>
        <Button BorderPane.alignment="CENTER" text="increment" onAction="#increment" />
    </left>
    <center>
        <!-- content from counter.fxml included here -->
        <fx:include fx:id="count" source="counter.fxml" />
    </center>
</BorderPane>
```

`OuterController`
The controller of the included fxml is injected to this controller. Here, the handler for the `onAction` event for the `Button` is used to increment the counter.

```java
package counter;

import javafx.fxml.FXML;

public class OuterController {

    // OuterController accesses state through similar CountState object.
    private CountState countState;

    public void initialize() {
        System.out.println("Current value: " + countState.getValue());
    }

    @FXML
    private void increment() {
        countState.increment();
    }
}
```
---

The biggest mistake teams make is they think that because different parts of the application need to communicate with each other (such as the start/end selection and the pathfinding display), they need to be in the same controller. Teams who master ECB/MVC will know that the state of the program is where this communication takes place, and separate controllers will make code much more maintainable and much more testable.




