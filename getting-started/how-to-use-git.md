## How to use Git

Before we start coding, let's do a quick walkthrough of how to use Git on your computer.

### 1. Setting up your repo

*If you're already familiar with Git, feel free to skip to step 2.*

Git is a version control system that makes it easier for multiple people to work on the same codebase. GitHub is a platform built on top of Git, and is widely used by open source teams to coordinate development.

If you're new to Git, we recommend downloading and installing [GitHub Desktop](https://desktop.github.com/). You can also use the Git CLI, but for this tutorial will assume you're using the desktop client.

Open GitHub Desktop and sign into your GitHub account. Once you get to the welcome page, click on the "Add a Local Repository" button. Select the `ubyssey.ca` folder from your filesystem.

![](/assets/add-local-repo.png)

You should then see a page that looks like this:

![](/assets/repo-page.png)

### 2. Creating a branch

Git uses a concept called branching to keep different versions of the codebase separated. Team members often work on their own "branch" to avoid code conflicts with others. You'll be creating a new branch to implement changes for the **Getting Started Tutorial** task.

Select `New Branch...` from the `Branch` dropdown menu:

![](/assets/new-branch-1.png)

**Choosing a branch name**

As a convention, we preface all branch names with their corresponding issue number, follow by a short yet descriptive name. For this task, use something like this:

![](/assets/new-branch-2.png)

### 3. Your first commit

Git watches your files and keeps track of all the changes you make to your code. The repo page in GitHub Desktop displays the current "status" of your branch, which includes a line-by-line change log of all your files. 

To demonstrate how this works, open `README.txt` and add your name to the top of the file. You should see something like this after you save the file:

![](/assets/first-commit-1.png)

When you are ready to save some changes, you have to create what's called a "commit." Every commit must contain a message that describes the changes being saved.

![](/assets/first-commit-2.png)

After you click "Commit to ...", the changes will disappear from the screen. This means that your changes have been saved and the working directory is "clean."

### 4. Publishing your branch

Now you're ready to publish the changes in your branch. Clicking the "Publish branch" button will synchronize your changes with GitHub's servers. It's a good idea to commit early and often in order to prevent losing your changes.

![](/assets/publish-branch.png)

Now if you go to https://github.com/ubyssey/ubyssey.ca, you should be able to see that your branch has been published!