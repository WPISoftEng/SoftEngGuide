# Scene Switching

###### View the example at https://github.com/WPISoftEng/JavaFXSceneSwitching

This by far is the concept that most students get wrong with their Soft Eng projects. With most problems in Soft Eng, the problem stems from students not understanding ECB/MVC. Rather than separating the Controller and the State, they fill their controllers with variables representing the state of the program. Is this text showing, the currently selected starting node, is a path being displayed, etc. By having these state variables as part of the controller, it makes it ***impossible*** to not hold on to the single instance of that controller, because if you do, you'll lose the state of the program. 

Instead, the state should be loaded *into* the controller when it is needed, and the controller transforms it into a view that the user can interact with. When the program is done using that controller, it may discard it, and load a new one in its place. The state objects will still hold the state of the program until they are called upon again later in the programs running.

Consider the following scene and its controller

`Scene1.fxml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<?import javafx.scene.control.Button?>
<?import javafx.scene.control.Label?>
<?import javafx.scene.layout.AnchorPane?>
<?import javafx.scene.text.Font?>
<AnchorPane maxHeight="-Infinity" maxWidth="-Infinity" minHeight="-Infinity" 
minWidth="-Infinity" prefHeight="400.0" prefWidth="600.0" 
xmlns="http://javafx.com/javafx/10.0.2-internal" 
xmlns:fx="http://javafx.com/fxml/1" 
fx:controller="edu.wpi.teamname.controllers.Scene1">
   <children>
      <Label layoutX="140.0" layoutY="140.0" text="Scene 1">
         <font>
            <Font size="96.0" />
         </font>
      </Label>
      <Button layoutX="240.0" layoutY="280.0" mnemonicParsing="false" onAction="#advanceScene"
        prefHeight="50.0" prefWidth="120.0" text="Advance Scene"/>
   </children>
</AnchorPane>

```

> Scene2.fxml looks identical, with the number changed

`Scene1.java`

```java
package edu.wpi.teamname.controllers;

import edu.wpi.teamname.App;
import java.io.IOException;
import javafx.event.ActionEvent;
import javafx.fxml.FXML;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;

public class Scene1 {

  @FXML
  private void advanceScene(ActionEvent e) {
    try {
        App.setRoot(View.Stage2);
    } catch (IOException ex) {
      ex.printStackTrace();
    }
  }
}
```

`App.java`

```java
public class App extends Application {
...
    public enum View 
    {
        Scene1("views/scene1.fxml"),
        Scene2("views/scene2.fxml");
     
        private String path;
     
        Environment(String path) {
            this.path = path;
        }
     
        public String getPath() {
            return path;
        }
    }
    ...
    public void setRoot(View view) {
        primaryStage.getScene().setRoot(FXMLLoader.load(getClass().getResource(view.getPath()).toExternalForm()));
    }
}
```
When the `Button` is clicked, `advanceScene` is called. `AdvanceScene` will tell App to change the scene, using a predefined list of scenes stores as an `enum`.

When the root changes, there will be no more references to the Scene1 controller will disappear, and the class will be destroyed. This is why it is important to save the state *not* in the controller. It will not persist.

This is of course better than the alternative that many, many teams try, where the scenes are all loaded beforehand, possible with their controllers as well, and are stored in the `App` class with getters abound.


```java
public class App {
    private Parent scene1;
    private Parent scene2;
    
    public void init() {
        Scene1 = FXMLLoader.load(...);
        Scene2 = FXMLLoader.load(...);
    }
    
    public Parent getScene1() { return scene1; }
    public Parent getScene2() { return scene2; }
    
    public void goToScene1() {primaryStage.getScene().setRoot(scene1);}
    public void goToScene2() {primaryStage.getScene().setRoot(scene2);}
}
```

This is clearly much messier, and leads to poor coding habits. **Use ECB!**
