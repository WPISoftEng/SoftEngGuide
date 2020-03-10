# Setting Up Git(Hub)

## Setting Up GitHub

> Create a [GitHub](github.com) account if you don't already have one

### Join the Organization

To collaborate easily with a team, GitHub has a team feature called 'Organizations'. Either Prof. Wong or an experienced SA will create organizations for all the teams. They will be in the form CS3733-C20-Team-A, with the term and year changed, and the team letter changed. Have the lead software engineer contact Prof. Wong or the SA in charge with their GitHub username, and they will add them. The leads should then add the rest of the team members.

### Setting Up the Project Repository

1. Visit [https://github.com/WPISoftEng/CS3733-Starter-Code](https://github.com/WPISoftEng/CS3733-Starter-Code)
2. Click 'Use This Template'
    ![](https://help.github.com/assets/images/help/repository/use-this-template-button.png)
3. Change the owner to be your team organization. Give the repo an interesting, yet informative name (Kiosk, Hospital App, etc)
4. Disable Merge and Rebase Commits
   1. **Settings > Options > Merge Button** > Uncheck *Allow Merge Commits*, *Allow Rebase Commits*
   2. This forces all merges to be 'squashed' which will make the history of the repo *much* cleaner, and make diagnosing bugs easier.
5. Add Review Requirements
   1. Select the following options
   ![](https://i.ibb.co/xM0GYxc/image.png)

## Setting Up Git

### Installation

#### Windows

1. Download *git for windows* [here](https://git-scm.com/download/win)
   - During the installation, be sure to install **Git for Bash**

#### Mac

`$ brew install linux`

Once you've installed Git, open the terminal

```bash
$ git config --global user.name "Your name here"
$ git config --global user.email "your_email@example.com"
```

> :memo: To cache your passwords, follow the instructions [here](https://help.github.com/en/github/using-git/caching-your-github-password-in-git)

### Getting a copy of the repo

In order to prevent cross-contamination caused by a team member pushing to your branch, the best way to clone the project is to fork the repository, then push to that repository when you've made a change to the code, and create pull requests from your fork. This prevents other team members from pushing to your branch, causing issues (which I have seen in the past). You can find info on forking repositories [here](https://help.github.com/en/articles/fork-a-repo). Once you've forked the repository, clone the **team** repository, and run the following command in the terminal 

    ```bash
    $ git remote --set-url --push https://github.com/<your_username>/<repo_name>.git
    ```

This lets you pull from upstream (the org repo), but push downstream (your fork)

### Git Hooks

Git hooks allow scripts to be run when different aspects of the git workflow are triggered. Some key triggers are pre-commit, and pre-push. As you might guess, these scripts would run before any changes are committed (after typing `git commit` but before any changes are actually committed) and before a push is made to a remote.  We can use these hooks to accomplish a couple of tasks
- Before a commit, it's nice to format code. This means that every commit will have good looking code, which prevents the need for future commits to be simple reording of imports, whitespace changes, etc. It also helps when reviewing code to not have to look through a bunch of whitespace errors.
- Before pushing, it's always a good idea to test your code. While TravisCI will test code automatically when a pull request is made, it's much faster to have the test run on your machine, and make changes locally than to wait for the travis report. 
Some basic webhooks are included in the starter code at `/.hooks/`. To enable these, simple run the command from the project root directory.

    ```bash
    $ git config core.hooksPath .hooks/
    ```
This will make the `.hooks/`` directory the path that git looks for hooks for this repository only, so you don't need to worry about it messing with other repositories.
