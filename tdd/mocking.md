# Mocking with Mockito

> Be sure to read about dependency injection first

Just how dependency injection takes away the pain of creating dependency graphs and properly creating dependencies in applications, mocking these dependencies makes writing tests much easier. By turning dependencies into mock objects, you don't need to worry about their creation or side effects, and you can focus solely on the class you're testing.



Here's an example (simplified from the Examples branch of the starter code)

```java
public class HomeController implements Initializable {

  @Inject DatabaseService db;
  @Inject ServiceTwo two;

  public void buttonClicked() {
   two.setEmployee(db.getEmployeeName());
  }
}
```

Here we have a class `HomeController` that has two dependencies, a `DatabaseService` and a `ServiceTwo`. When a button is clicked, it calls `two.setEmployee` with the result of a call to `db.getEmployeeName` . If we were testing either of those services, we might be concerned with what `setEmployee` does, or what `getEmployeeName` returns. Since we are testing `HomeController`, all we care about is that the buttonClicked method did what it was supposed to, i.e it invoked `two.setEmployee` with the result of `db.getEmployeeName()` regardless of the implementations of those methods. 

## Mocking

To properly test this class, we will *mock* the dependencies. Mocking means we will inject special, fake, dependencies that come with additonal features so we can monitor how they are used, and control what happens when methods are called upon them. Let's set up a simple mock

```java
// TestHomeController.java
@ExtendWith(MockitoExtension.class)
public class HomeTest {

  @Mock ServiceTwo two;
  @Mock DatabaseService one;

  @InjectMocks HomeController controller;

  @BeforeEach
  public void init() {
  }

  @Override
  public void testButtonClick {
   controller.buttonClicked();
  }
}
```

### Mock Injections

To mock a dependency, simply declare it with the `@Mock` annnotation. Then, for the object that is receiving the mocks, use `@InjectMocks`

Here we have a simple test that calls the buttonClicked method on the controller. There are no assertions, so it isn't a great test case, but it's about to be. The first step is to check if `two.setEmployee` was called. 

```java
@Override
  public void testButtonClick {
   controller.buttonClicked();
    verify(two).setEmployee(any());
  }
```

This checks if the `setEmployee` method was called with any argument passed in. If we run this test, it will pass. However, we want to make sure that the method was called specifically with the output of `getEmployeeName`. To do this, we'll *mock* the method to return an expected value, and then check that that value was the one used in the method call. 

```java
@Override
  public void testButtonClick {
    // Arrange
    when(db.getEmployeeName()).thenReturn("Expected Name");
    //Act
    controller.buttonClicked();
    //Assert
    verify(two).setEmployee("Expected Name");
  }
```

Now we can be sure that the `setEmployee` method is being called with the output of `getEmployeeName`



## Spy

Suppose you had a dependency that *could* be injected, and didn't need to be mocked. To get a real object, simple annotate with `@Spy` instead of `@Mock`. This object will still let you check the method calls, similar to how we checked `setEmployee`, but will not require you to mock methods.