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


