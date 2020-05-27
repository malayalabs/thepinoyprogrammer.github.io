---
layout: post
title: "Git for Beginners Series - (Part 1) Creating your first repository"
---

![header-image01](https://drive.google.com/file/d/1fQX0cWFgq_ZawsKGkdZqTIpOgRrEGCJj/view?usp=sharing)

> Have you heard of the tool Git?
> Of course you have!
> But why are you here?
> Maybe your just a beginner programmer?
> Or a college student and want to start a career in tech?
> Whatever your goal is, this tutorial aims to help you get started with Git if you haven't already.

### A little background on Git...

<a href="https://git-scm.com" target="_blank">Git</a> is a <a href="https://git-scm.com/about/free-and-open-source" target="_blank">free and open source</a> distributed version control system used for tracking changes in source code during software development. It is designed for coordinating work among programmers, but it can be used to track changes in any set of files.

<br />

### What is a version control system?

If you have played video games before, there is almost always a feature called "<a href="https://en.wikipedia.org/wiki/Saved_game" target="_blank">Save Game</a>". This enables the player (you) to save your progress so you can come back later and play the game where you left it. 

This is somewhat similar to a version control system. You can save your code as you make progress, keep track of changes, and the most important feature of version control: it lets you collaborate with other developers and enables you to work in parallel especially when you are working with a team and each member is working on their own task/feature.

<br />

### Installation

**Windows**

[https://git-scm.com/download/win](https://git-scm.com/download/win)

**Linux**

*Fedora (or any closely-related RPM-based distribution, such as RHEL or CentOS):*

```bash
$ sudo dnf install git-all
```

*If you’re on a Debian-based distribution, such as Ubuntu, try `apt`:*

```bash
$ sudo apt install git-all
```

**MacOS**

```bash
$ brew install git
```

<br />

### Basics

**NOTE:** In this basic tutorial, I recommend that you use <a href="https://stackoverflow.com/questions/17807485/what-is-the-exact-meaning-of-git-bash" target="_blank">Git Bash</a> if you're on Windows or just use the <a href="https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal" target="_blank">Terminal</a> if you're on Linux.

<b>1) Initializing a <a href="https://www.quora.com/What-is-the-meaning-of-%E2%80%98Git-repository%E2%80%99" target="_blank">git repository</a> in your local computer</b>

In this step we are going to start to get a feel on how to use `git` in your local machine. To get started, let's first create our project folder:

```bash
mkdir my-project-folder
```

Then let's initialize a `git` repository in the folder we just created.

*Remember, you can always replace 'my-project-folder' to any name you want.*

```bash
cd my-project-folder
git init
```

You should see a message similar to this:

<code>Initialized empty Git repository in /path/to/my-project-folder/.git/</code>

> Great! You've initialized your `git` repo. Now we can play around with git commands in the next steps...

**2) Add and Commit**

Now that we are inside our newly initialized `git` repo, let's create a file and save it to our project folder:

```bash
touch file.txt
```

After adding that file, try running:

```bash
git status
```

![image01](/images/2020-5-24/image01.png)

It should show you `file.txt` under **Untracked files** section. `Git` is basically saying that you have 1 file (`file.txt`) that you have modified or created in your folder (this usually highlighted in red). When doing `git status`, it will show you the current files that are either added or modified in your `git` repo. This all will make sense later once we add and commit changes to the repository.

Now let's add our file:

```bash
git add file.txt
```

![image02](/images/2020-5-24/image02.png)

Let's run `git status` once again to see changes made to the repo. Now you should see that `file.txt` is under **Changes to be commited**. This means that the files you added using `git add` are ready to be commited to your repository.

*Now, you might ask what is the difference between `git add` and `git commit`?*

> **git add** is a command for marking files that are ready to be commited to your repository. This might be useful when you have multiple changes on your files but are not ready to commit them all. Which leaves us to the next definition...

> **git commit** is a command for saving the created/modified files marked with `git add` to your repository. Remember when we talked about saving your progress in a video game? Well, this is pretty similar. 

Nice! We're making good progress. Here's a quick summary of what we've done so far:

- Created our first git repository folder.
- Added our first file to our project folder.
- Commited the file to our git repository.

That's looks great right? Now on the next steps you're going to learn how to setup your Github account to publish your repository on the cloud.

<br />

**3) Creating a GitHub repository**

So far, if we are playing a video game right now, we have saved our progress in our local computer. But what if your hard drive gets corrupted or some unfortunate event caused your progress to be deleted? (let's not wish or think about it lol) You better be ready for the worst. So what do we do now?

<a href="https://github.com" target="_blank">GitHub</a> to the rescue! GitHub is a simply a platform for uploading/downloading git repositories. It makes it easy for you to push your changes to the cloud. And it's FREE to register an account and create unlimited repositories.

**Steps on how to create a GitHub repository:**

- Register for a GitHub account here: <a href="https://github.com/join" target="_blank">https://github.com/join</a>

- Create a new repository

  - On the top right corner of the screen, click the '+' sign then choose 'New repository':

    ![image03](/images/2020-5-24/image03.png)

  - Enter repository name:

    ![image04](/images/2020-5-24/image04.png)

  - Click the 'Create repository' button

- Clone repository

  After creating your new empty repository, you should see a screen like this:

  ![image05.png](/images/2020-5-24/image05.png)

  Choose the 2nd option where it says *'...push an existing repository from the command line'*

  ![image06.png](/images/2020-5-24/image06.png)

  ***Note: Make sure you are inside your project folder where you have initialized your git repository***

<br />

*Feel free to contact us at [thepinoyprogrammer@protonmail.com](mailto:thepinoyprogrammer@protonmail.com) if you have further questions.*

<br />

### Next: Git Fundamentals - (Part 2) Push and Pull from your GitHub repository
