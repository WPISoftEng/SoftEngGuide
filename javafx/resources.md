# Accessing Resources

One of the hardest parts about JavaFX is accessing files that your program will use, such as images or CSVs.
To access these files, we use the `ClassLoader::getResourceAsStream` method.

## Resources Folder

The starter code configures your project with two similar folder structures, one for source code files (`src/main/java`, and one for resources (`src/main/resources`.
These folders should remain similar in structure, with regards to packages. 

## Relative Path

Suppose you have a controller class in the `src/main/java/edu.wpi.teamZ.controllers` package, and an Image `blue.png` that you want to access in `src/main/resources/edu/wpi/teamZ/views/blue.png`.
The path to the image relative to the Controller class is `../views/blue.png`. 
So we access it as such:

*From controller class*

```java
... = getClass().getResourceAsStream('../views/blue.png');
```

If the image was at `.../teamZ/controllers/blue.png`, then you could access it more simply with

```java
... = getClass().getResourcesAsStream('blue.png');
```

If you're accessing files using any other method than this, your application **will break** when it comes time to jar it up.

## Saving Files

You should save files to AppData or the desktop.
Saving to a file relative to your project (such as `src/main/resources/...` will break during the jar run.
Asking the user where to save a file using a dialog box is always a good solution.