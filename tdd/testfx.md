# TestFX

## Getting Started with TestFX

The TestFX docs are some of the most unkept, hard-to-navigate, feature-missing docs I've ever seen. It sucks, because there doesn't seem to be another library that does what TestFX does. So, we shall make due. 

In the preconfigured `build.gradle` are the testfx dependencies that you will need

```java
plugins {
    id 'org.openjfx.javafxplugin' version '0.0.7'
}

dependencies {
    testImplementation(
        'org.junit.jupiter:junit-jupiter:5.5.0',
        'org.testfx:testfx-junit5:4.0.15-alpha'
    )
}
```

## Test Single Views

If you were to test your application without unit tests, you would need to run the application, wait for everything to initialize, and then click your way through the app to set it up with the correct settings, and then manually do the verification. This is a time consuming process, is not easily repeatable, and you are likely to forget to test every feature every time you make a change.

We can write tests to go through this process of loading up the app and clicking through like a human would, but using TestFX we can pull out single *units* of our application and test them individually.

The smallest unit we'll break our program into will be a single view and its corresponding controller. This lets us test the entire controller class, but no more.

## Setting up Tests

The starter code contains a single FXML view. Let's walk through creating a new test case for this FXML.

> :information_source: All of the code can be found in the `TDD-Demo` repo in the WPISoftEng github

### Creating the test class

Begin by creating a new class in the same package as the controller, but in the `tests` folder.

```java
import static org.testfx.api.FxAssert.verifyThat;
import static org.testfx.matcher.control.LabeledMatchers.hasText;

import org.testfx.framework.junit.ApplicationTest;
import org.testfx.robot.Motion;

import javafx.scene.control.Button;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.stage.Stage;

import org.junit.Test;

public class HomeViewTest extends ApplicationTest {
    @Override public void start(Stage stage) throws IOException {
        Parent sceneRoot = FXMLLoader.load(getClass().getResource("HomeView.fxml"))
        Scene scene = new Scene(sceneRoot);
        stage.setScene(scene);
        stage.show();
    }
}
```

Our HomeView is a simple scene with a button and some text. The controller is set up to toggle the visibility of the text each time you click the button.

### Simulate Button Presses

This will be one of the most common use cases you will test, since the application will be mostly made of buttons.

Here is a simple test case that clicks on the button with text 'Click Me', and verifies that the node with fx:id `text` is visible.

```java
@Test
public void testSingleClick() {
    clickOn('Click Me');
    verifyThat('#text', Node::isVisible);
}
```
