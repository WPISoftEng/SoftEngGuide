# Responsiveness (Or Lack Thereof)

Responsiveness is not an easy task. When doing web development there are lots of frameworks that make it easier to accomplish, but not many applications are written in java, so there isn't a whole lot of variety.

In my experience, responsiveness with JavaFX is what slows down a project the most. It's very easy to hard code the width, height, and layout of elements in a scene, but relating them to each other and using percentages to make them look the same on different devices is not as simple as it should be.

For this reason, my recommendation is to choose a specific size (1920x1080) for the application and hardcode everything else. It will look good when displayed on the screens in WPI Lecture Halls, and most laptop screens are big enough to view it. If you work on a desktop at home, it will also fit perfectly on a full HD monitor.

## Absolute Layout

In order to achieve an absolute layout (positioning elements on the screen via absolute pixel values, rather than relative to size of screen or other elements) you need to use an AnchorPane as the root object.

Then you can create objects and place them on the anchorpane in specific locations by defining their anchor points.
