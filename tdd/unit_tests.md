# Unit Tests

When testing applications, there are two methods of testing, unit testing, and integration testing. 

Unit Testing involves testing each individual part, or unit, of the application to make sure it works as it should. 

Integration Testing is testing to ensure the units work correctly together.

In this class, try to focus mainly on writing unit tests. There will probably not be enough time to write both unit tests *and* integration tests, so the testing for how the different pieces fit together will mostly be done through running the application on your desktop and trying things out. If you find that you do have time to write more advanced tests, you can move on to integration tests.

For both unit tests and integration tests, you'll use TestFX.



## What goes into a good test

I learned during my Amazon internship that every test case has 3 parts, **Arrange**, **Act**, **Assert**. 

**Arrange** - This is where you set up the test case. Arrange any object setup, variable initalization, etc.

**Act** - Perform some action. Usually a method call.

**Assert** - Verify the intended result. Check 'actual' outcome against a pre-configured 'expected' outcome.