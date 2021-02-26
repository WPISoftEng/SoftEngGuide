# Controller Constructors

Sometimes it's necessary to create an fxml view that changes at runtime, but may not be linked to an easily accessible model.
An example is if you created an FXML file to visualize the details of a node.
In this case, we can't just hardcode the details, as they will change for each node.
We will also have many instances of this fxml view and controller, so we will not be able to use a singleton for the model.
To customize each controller for each underlying model, we need to pass in the model via the constructor when we create the controller.

This is the first step to proper dependency injection.

## An Example

In this example we are going to create a list of student details. 

```java

public class Student {
  private SimpleStringProperty id;
  private SimpleIntegerProperty age;
  private SimpleStringProperty name;
}
```

```java
private class StudentViewController implements Initializable{
  @FXML private Label idLabel;
  @FXML private Label ageLabel;
  @FXML private Label nameLabel;

  private Student student;

  public StudentViewController(Student student) {
    this.student = student;
  }

  public void initialize(URL location, ResourceBundle resources) {
    this.idLabel.textProperty().bind(this.student.id);
    this.ageLabel.textProperty().bind(this.student.age);
    this.nameLabel.textProperty().bind(this.student.name);
  }
}
```

```java
private class MainViewController {
  @FXML private VBox vbox;
  private List<Student> students;

  ...

  public void initialize() {
    for (Student student: students) {
      FXMLLoader studentControllerLoader = new FXMLLoader(getClass().getResourceAsStream("studentView.fxml"));
      studentControllerLoader.setControllerFactory(StudentViewController -> new StudentViewController(student));
      vbox.getChildren().add(studentControllerLoader.load());
    }
  }
}
```

## What not to do

It may be very tempting to make methods in the controller public, access the instance of the controller, and then set the controller properties view the public methods.
This is not proper MVC/ECB, and should not be used.
The reason you don't want to write your application this way is because you now have a controller that is dependant on another controller for data, rather than the model.
