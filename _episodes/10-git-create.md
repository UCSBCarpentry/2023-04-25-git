---
title: Creating a Repository
teaching: 10
exercises: 0
questions:
- "Where does Git store information?"
objectives:
- "Create a local Git repository."
- "Describe the purpose of the `.git` directory."
keypoints:
- "`git init` initializes a repository."
- "Git stores all of its repository data in the `.git` directory."
---

Once Git is configured, we can start using it. The usage we will demonstrate is making a respository for a personal website. Later, we'll show how you can publish 
this site on GitHub pages.

First, let's create a directory in `Desktop` folder for our work and then move into that directory:

~~~
$ cd ~/Desktop
$ mkdir simple-site
$ cd simple-site
~~~
{: .language-bash}

Then we tell Git to make `simple-site` a [repository]({{ page.root }}{% link reference.md %}#repository)
-- a place where Git can store versions of our files:


~~~
$ git init
~~~
{: .language-bash}

It is important to note that `git init` will create a repository that
includes subdirectories and their files---there is no need to create
separate repositories nested within the `simple-site` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `simple-site` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~
{: .language-bash}

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `simple-site` called `.git`:

~~~
$ ls -a
~~~
{: .language-bash}

~~~
.	..	.git
~~~
{: .output}

Git uses this special subdirectory to store all the information about the project,
including all files and sub-directories located within the project's directory.
If we ever delete the `.git` subdirectory,
we will lose the project's history.

Next, we will change the default branch to be called `main`.
This might be the default branch depending on your settings and version
of git.
See the setup episode for more information on this change.

~~~
# only necessary if default branch is 'master'
git checkout -b main
~~~
{: .language-bash}
~~~
Switched to a new branch 'main'
~~~
{: .output}


We can check that everything is set up correctly
by asking Git to tell us the status of our project:

~~~
$ git status
~~~
{: .language-bash}
~~~
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
~~~
{: .output}

If you are using a different version of `git`, the exact
wording of the output might be slightly different.
