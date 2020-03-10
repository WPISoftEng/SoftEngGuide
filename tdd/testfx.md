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

## Testing A View

If you were to test your application without unit tests, you would need to run the application, wait for everything to initialize, and then click your way through the app to set it up with the correct settings, and then manually do the verification. This is a time consuming process, is not easily repeatable, and you are likely to forget to test every feature every time you make a change.

We can write tests to go through this process of loading up the app and clicking through like a human would, but using TestFX we can pull out single *units* of our application and test them individually.
