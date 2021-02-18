# Storing State

When using EBC or MVC design patterns, it's very crucial that you properly split up the Controllers and the Entities/Models.
With JavaFX, it's easy to separate the View and the Controllers, since you're using FXML files, and they can only hold the views. Where students always go wrong is how they store the state of the program. 

## Creating a State Class

The first step into storing state correctly is having classes that represent the state you're trying to store.
These can be POJO's, or they might be a little more complex if they act as caches for the database to hold the state more permanently, but what I want to focus on more is the relationship between the state class and the FXML Controllers. 


### An Example
Let's say you have a two-view application that contains only a single text field and a button in the first view. When you press the button, the application will switch to a new view that contains only a label, with the text of the label being the same as the text that was in the text field.

An inexperienced programmer might try something like this:

```java
private void onButtonClick() {
  FXMLLoader fxmlLoader = new FXMLLoader();
  Parent p = fxmlLoader.load(getClass().getResourceAsStream("foo.fxml"));
  SceneTwoController s2Controller = (SceneTwoController) fxmlLoader.getController();
  s2Controller.setLabelText(this.textField.getText());
  ...
}
```

They use the FXMLLoader to gain access to the instance of the Controller that controls the next view, and set the text of the label directly throught the controller.
This is a **terrible** design decision. There has *very high* **coupling**[^1] between the controller of this view and the controller of the second view.

### A better way

Let's introduce a state class into our example.
The state class is a simple java object that will hold the text that was entered into our text field.

```java
public class TextState {
  private String text;
  
  public void setString(...) {...}
  
  public String getString() {...}
```

Now that we have this state class, there is no need for the controllers to talk to each other directly.
Instead, **they communicate through state**.
JavaFX has some advanced features that allow this communication to happen automatically via [properties and bindings](https://edencoding.com/javafx-properties-and-binding-a-complete-guide/).

The last step we need is for the controllers to share the state object.
This can be accomplished through [dependency injection of a singleton](../di/singleton.md).


[^1]: Coupling is a measure of how dependent one module is on another. Good software has **low coupling**
