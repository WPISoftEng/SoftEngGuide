# IntelliJ IDEA

## Installing IntelliJ

### Apply for Student Discount

In order to use the Database tools that come with IntelliJ, we need the Ultimate edition. This requires us to have a student license to get it for free. Apply [here](https://www.jetbrains.com/shop/eform/students)

Once you've received your student discount, visit your account page, and download and install **IntelliJ IDEA Ultimate Edition**. 

## Setting up IntelliJ

Once you've opened IntelliJ for the first time, you'll need to configure some settings. Customize it however you like.

### Import Project

When you get the the main start screen, select **Import Project**.

> :information_source: Even though we are using a Version Control System (git), we don't want to checkout from IntelliJ. To properly learn how to use git, it is more useful to learn the CLI (Command Line Interface) than to use the GUI that is specific to JetBrains.

Navigate to where you cloned your repository. Select the root folder of the repository (the folder that contains build.gradle) and select **open**. 
When asked if you want to create project from existing source or import from model, select model, and choose **Gradle**. 
On the next screen, select **Use auto import**. That should be the only setting you need to change.

### Configuring Project Structure

**File > Project Structure**

- Project - Make sure both the SDK and Language Level are set to 13
- Modules - Expand the `src` folder and both the `main` and `test` subfolders. 
  - Mark **src/main/java** as 'Sources'
  - Mark **src/test/java** as 'Tests'
  - Mark **src/*/resources** folder as 'Resources'
