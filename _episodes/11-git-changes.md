---
title: Tracking Changes
teaching: 20
exercises: 0
questions:
- "How do I record changes in Git?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"
objectives:
- "Go through the modify-add-commit cycle for one or more files."
- "Explain where information is stored at each stage of that cycle."
- "Distinguish between descriptive and non-descriptive commit messages."
keypoints:
- "`git status` shows the status of a repository."
- "Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded)."
- "`git add` puts files in the staging area."
- "`git commit` saves the staged content as a new commit in the local repository."
- "Write a commit message that accurately describes your changes."
---

## Recall Bash Commands

First let's make sure we're still in the right directory. The first type of shell script we are using is called user input.
So, we still need a subdirectory, `user-input`, inside of `shell-scripts`.
Confirm you have `~/Desktop/shell-scripts/user-input` and that it's your working directory.
If you are not inside `~/Desktop/shell-scripts/user-input`, recall how to move to through directories and how to make a subdirectory.

## Recall Bash Commands

Try to create an empty directory inside of `shell-scripts` called `user-input.`

#### Commands
To see what your working directory is:
~~~
$ pwd
~~~
{: .language-bash}

To change directories:
~~~
$ cd ~/Desktop/shell-scripts
~~~
{: .language-bash}

To create a directory, and then move to that directory:
~~~
$ mkdir user-input # user-input is a chosen name for our subdirectory
$ cd user-input
~~~
{: .language-bash}

Let's create a shell script file inside of `~desktop/shell-scripts/user-input` that is called `ui-example.sh` that contains shell scripts using user input.
We'll use `nano` to edit the file;
you can use whatever editor you like.
In particular, this does not have to be the `core.editor` you set globally earlier. But remember, the bash command to create or edit a new file will depend on the editor you choose (it might not be `nano`). For a refresher on text editors, check out ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) in [The Unix Shell](https://swcarpentry.github.io/shell-novice/) lesson.

~~~
$ nano ui-example.sh
~~~
{: .language-bash}

Type the text below into the `ui-example.sh` file. Save the script; recall or discuss what each command does.

~~~
echo "What is your name?"
read name
echo "Hello $name."
~~~
{: .output}

Let's first verify that the file was properly created by running the list command (`ls`):


~~~
$ ls
~~~
{: .language-bash}

~~~
ui-example.sh
~~~
{: .output}


`ui-example.sh` contains some lines of code, which we can see by running:

~~~
$ cat ui-example.sh
~~~
{: .language-bash}

~~~
echo "What is your name?"
read name
echo "Hello $name."
~~~
{: .output}

Run `ui-example.sh` using the `bash` command. What do you think the `read` command does?

If we check the status of our project again,
Git tells us that it's noticed the new file:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main

No commits yet

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	ui-example.sh

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

~~~
$ git add ui-example.sh
~~~
{: .language-bash}

and then check that the right thing happened:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   ui-example.sh

~~~
{: .output}

Git now knows that it's supposed to keep track of `ui-example.sh`,
but it hasn't recorded these changes as a commit yet.
To get it to do that,
we need to run one more command:

~~~
$ git commit -m "Adding in an example script on user input (commit #1)"
~~~
{: .language-bash}

~~~
[main (root-commit) f22b25e] Adding in an example script on user input (commit #1)
 1 file changed, 3 insertions(+)
 create mode 100644 ui-example.sh
~~~
{: .output}

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [commit]({{ page.root }}{% link reference.md %}#commit)
(or [revision]({{ page.root }}{% link reference.md %}#revision)) and its short identifier is `f22b25e`. Your commit may have another identifier.

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured as `core.editor`)
so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<50 characters) statement about the
changes made in the commit. Generally, the message should complete the sentence "If applied, this commit will" <commit message here>.
If you want to go into more detail, add a blank line between the summary line and your additional notes. Use this additional space to explain why you made changes and/or what their impact will be.

If we run `git status` now:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
nothing to commit, working directory clean
~~~
{: .output}

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

~~~
$ git log
~~~
{: .language-bash}

~~~
commit 8d4f409759ab29069329922c15d51ebf856aa065
Author: user <user@ucsb.edu>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Adding in an example script on user input (commit #1)
~~~
{: .output}

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the commit's author,
when it was created,
and the log message Git was given when the commit was created.

> ## Where Are My Changes?
>
> If we run `ls` at this point, we will still see just one file called `ui-example.sh`.
> That's because Git saves information about files' history
> in the special `.git` directory mentioned earlier
> so that our filesystem doesn't become cluttered
> (and so that we can't accidentally edit or delete an old version).
{: .callout}

Now suppose let's add a comment to `ui-example.sh`.
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)

~~~
$ nano ui-example.sh
$ cat ui-example.sh
~~~
{: .language-bash}

~~~
# 'read' command requests user to input text
echo "What is your name?"
read name
echo "Hello $name."
~~~
{: .output}

When we run `git status` now,
it tells us that a file it already knows about has been modified:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ui-example.sh

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
nor have we saved them (which we do with `git commit`).
So let's do that now. It is good practice to always review
our changes before saving them. We do this using `git diff`.
This shows us the differences between the current state
of the file and the most recently saved version:

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/user-input/ui-example.sh b/user-input/ui-example.sh
index b5c9f2a..6db94fd 100644
--- a/user-input/ui-example.sh
+++ b/user-input/ui-example.sh
@@ -1,3 +1,4 @@
+#'read' command requests user to input text
 echo "What is your name?"
 read name
 echo "Hello $name."
~~~
{: .output}

The output is cryptic because
it is actually a series of commands for tools like editors and `patch`
telling them how to reconstruct one file given the other.
If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file
    Git is comparing;
    `df0654a` and `315bf3a` are unique computer-generated labels for those versions.
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` marker in the first column shows where we added a line.

After reviewing our change, it's time to commit it:

~~~
$ git commit -m "Add comment on what read command does (commit #2)"
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ui-example.sh

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that:

~~~
$ git add ui-example.sh
$ git commit -m "Add comment on what read command does (commit #2)"
~~~
{: .language-bash}

~~~
[main 34961b1] Add comment on what `read` command does (commit #2)
 1 file changed, 1 insertion(+)
~~~
{: .output}

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.
For example,
suppose we're adding a few citations to relevant research to our thesis.
We might want to commit those additions,
and the corresponding bibliography entries,
but *not* commit some of our work drafting the conclusion
(which we haven't finished yet).

To allow for this,
Git has a special *staging area*
where it keeps track of things that have been added to
the current [changeset]({{ page.root }}{% link reference.md %}#changeset)
but not yet committed.

> ## Staging Area
>
> If you think of Git as taking snapshots of changes over the life of a project,
> `git add` specifies *what* will go in a snapshot
> (putting things in the staging area),
> and `git commit` then *actually takes* the snapshot, and
> makes a permanent record of it (as a commit).
> If you don't have anything staged when you type `git commit`,
> Git will prompt you to use `git commit -a` or `git commit --all`,
> which is kind of like gathering *everyone* to take a group photo!
> However, it's almost always better to
> explicitly add things to the staging area, because you might
> commit changes you forgot you made. (Going back to the group photo simile,
> you might get an extra with incomplete makeup walking on
> the stage for the picture because you used `-a`!)
> Try to stage things manually,
> or you might find yourself searching for "git undo commit" more
> than you would like!
{: .callout}

![The Git Staging Area](../fig/git-staging-area.svg)

Let's watch as our changes to a file move from our editor
to the staging area
and into long-term storage.
First, move the comment to be placed below `Hello $name`. Add in more content to the comment.:

~~~
$ nano ui-example.sh
$ cat ui-example.sh
~~~
{: .language-bash}

~~~
echo "What is your name?"
read name
echo "Hello $name."
#the read command requests user input, we use `$` to recall the variable established by `read`
~~~
{: .output}

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/user-input/ui-example.sh b/user-input/ui-example.sh
index 6db94fd..d586f12 100644
--- a/user-input/ui-example.sh
+++ b/user-input/ui-example.sh
@@ -1,4 +1,4 @@
-#'read' command requests user to input text
 echo "What is your name?"
 read name
 echo "Hello $name."
+#`read` command requests user input, we use `$` to recall the variable established by `read`
~~~
{: .output}

So far, so good:
we've added one line to the end of the file
(shown with a `+` in the first column).
Now let's put that change in the staging area
and see what `git diff` reports:

~~~
$ git add ui-example.sh
$ git diff
~~~
{: .language-bash}

There is no output:
as far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory.
However,
if we do this:

~~~
$ git diff --staged
~~~
{: .language-bash}

~~~
diff --git a/user-input/ui-example.sh b/user-input/ui-example.sh
index 6db94fd..d586f12 100644
--- a/user-input/ui-example.sh
+++ b/user-input/ui-example.sh
@@ -1,4 +1,4 @@
-#'read' command requests user to input text
 echo "What is your name?"
 read name
 echo "Hello $name."
~~~
{: .output}

it shows us the difference between
the last committed change
and what's in the staging area.
Let's save our changes:

~~~
$ git commit -m "Adding more notes about read (commit #3)"
~~~
{: .language-bash}

~~~
[main 005937f] "Adding more notes about read command (commit #3)"
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .output}

check our status:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
nothing to commit, working directory clean
~~~
{: .output}

and look at the history of what we've done so far:

~~~
$ git log
~~~
{: .language-bash}

~~~
commit d2f536d6198f12ebba1849af49405a4f54845dd6 (HEAD -> main)
Author: user <user@ucsb.edu>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Adding more notes about read command (commit #3)

commit b798ee602bd76b92c39d8c62c6637d36109365e4
Author: user <user@ucsb.edu>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add comment on what read command does (commit #2)

commit 8d4f409759ab29069329922c15d51ebf856aa065
Author: user <user@ucsb.edu>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Adding in an example script on user input (commit #1)
~~~
{: .output}

> ## Word-based diffing
>
> Sometimes, e.g. in the case of the text documents a line-wise
> diff is too coarse. That is where the `--color-words` option of
> `git diff` comes in very useful as it highlights the changed
> words using colors.
{: .callout}

> ## Paging the Log
>
> When the output of `git log` is too long to fit in your screen,
> `git` uses a program to split it into pages of the size of your screen.
> When this "pager" is called, you will notice that the last line in your
> screen is a `:`, instead of your usual prompt.
>
> *   To get out of the pager, press <kbd>Q</kbd>.
> *   To move to the next page, press <kbd>Spacebar</kbd>.
> *   To search for `some_word` in all pages,
>     press <kbd>/</kbd>
>     and type `some_word`.
>     Navigate through matches pressing <kbd>N</kbd>.
{: .callout}

> ## Limit Log Size
>
> To avoid having `git log` cover your entire terminal screen, you can limit the
> number of commits that Git lists by using `-N`, where `N` is the number of
> commits that you want to view. For example, if you only want information from
> the last commit you can use:
>
> ~~~
> $ git log -1
> ~~~
> {: .language-bash}
>
> ~~~
> commit 005937fbe2a98fb83f0ade869025dc2636b4dad5 (HEAD -> main)
> Author: user <user@ucsb.edu>
> Date:   Thu Aug 22 10:14:07 2013 -0400
>
>    Adding more notes about read command (commit #3)
> ~~~
> {: .output}
>
> You can also reduce the quantity of information using the
> `--oneline` option:
>
> ~~~
> $ git log --oneline
> ~~~
> {: .language-bash}
> ~~~
> 005937f (HEAD -> main) Adding more notes about read command (commit #3)
> 34961b1 Add comment on what `read` command does (commit #2)
> f22b25e Adding in an example script on user input (commit #1)
> ~~~
> {: .output}
>
> You can also combine the `--oneline` option with others. One useful
> combination adds `--graph` to display the commit history as a text-based
> graph and to indicate which commits are associated with the
> current `HEAD`, the current branch `main`, or
> [other Git references][git-references]:
>
> ~~~
> $ git log --oneline --graph
> ~~~
> {: .language-bash}
> ~~~
> * 005937f (HEAD -> main) Adding more notes about read command (commit #3)
> * 34961b1 Add comment on what `read` command does (commit #2)
> * f22b25e Adding in an example script on user input (commit #1)
> ~~~
> {: .output}
{: .callout}


## Directories in Git
Two important facts you should know about directories in Git:

1.   Git does not track directories on their own, only files within them.
2.   If you can create a directory in your Git repository and populate it with files,
     you can add all the files in the directory at once.  

Create a new directory named 'loops'
We will be using this directory in the following lessons so please follow along.

~~~
$ cd ..
$ mkdir loops
$ git status
$ git add loops
$ git status
~~~
{: .language-bash}

Note, our newly created empty directory `loops` does not appear in
the list of untracked files even if we explicitly add it (_via_ `git add`) to our
repository. This is the reason why you will sometimes see `.gitkeep` files
in otherwise empty directories. Unlike `.gitignore`, these files are not special
and their sole purpose is to populate a directory so that Git adds it to
the repository. In fact, you can name such files anything you like.

Add multiple files to a directory at once.  
Try adding a new for-loop.sh file into your new subdirectory

~~~
git add <directory-with-files>
~~~
{: .language-bash}

Try it for yourself:

~~~
$ touch loops/for-loop.sh
$ git status
$ git add loops
$ git status
~~~
{: .language-bash}

Before moving on, we will commit these changes.

~~~
$ git commit -m "Add in subdir for loops"
$ cd loops
~~~
{: .language-bash}

To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

![The Git Commit Workflow](../fig/git-committing.svg)

> ## Committing Changes to Git
>
> Which command(s) below would save the changes of `myfile.txt`
> to my local Git repository?
>
> 1. ~~~
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 2. ~~~
>    $ git init myfile.txt
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 3. ~~~
>    $ git add myfile.txt
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 4. ~~~
>    $ git commit -m myfile.txt "my recent changes"
>    ~~~
>    {: .language-bash}
>
> > ## Solution
> >
> > 1. Would only create a commit if files have already been staged.
> > 2. Would try to create a new repository.
> > 3. Is correct: first add the file to the staging area, then commit.
> > 4. Would try to commit a file "my recent changes" with the message myfile.txt.
> {: .solution}
{: .challenge}

#### Committing Multiple Files

The staging area can hold changes from any number of files
that you want to commit as a single snapshot. The goals for the rest of this lesson are:

1. Add some text to `for-loop.sh`
2. Create a new file `notes-for-loop.txt`
3. Add changes from both files to the staging area, and commit those changes.

First we make our changes to the for-loop.sh` files:
~~~
$ nano for-loop.sh
$ cat for-loop.sh
~~~
{: .language-bash}
~~~
# for-loop example
# this for loop outputs numbers
~~~
{: .output}

~~~
$ nano notes-for-loop.txt
$ cat notes-for-loop.txt
~~~
{: .language-bash}
~~~
For loops start with `for` and ends with `done`.
~~~
{: .output}

Now you can add both files to the staging area. We can do that in one line:

~~~
$ git add for-loop.sh notes-for-loop.txt
~~~
{: .language-bash}
Or with multiple commands:
~~~
$ git add for-loop.sh
$ git add notes-for-loop.txt
~~~
{: .language-bash}
Now the files are ready to commit. You can check that using `git status`. If you are ready to commit use:
~~~
$ git commit -m "Add in for loop example"
~~~
{: .language-bash}
~~~
[main 7f9e498] add in for loop example
2 files changed, 3 insertions(+)
create mode 100644 for-loop.sh
create mode 100644 notes-for-loop.txt
~~~

Add in and commit the script of the for-loop example.
Think about how the structure of this for loop works.

~~~
# for-loop example
# this for loop outputs numbers

for variable in 1 2 3 4 5
do
  echo "output number $variable"
done
~~~
{: .language-bash}


> ## `bio` Repository
>
> * Create a new Git repository on your computer called `bio`.
> * Write a three-line biography for yourself in a file called `me.txt`,
> commit your changes
> * Modify one line, add a fourth line
> * Display the differences
> between its updated state and its original state.
>
> > ## Solution
> >
> > If needed, move out of the `shell-scripts` folder:
> >
> > ~~~
> > $ cd ..
> > ~~~
> > {: .language-bash}
> >
> > Create a new folder called `bio` and 'move' into it:
> >
> > ~~~
> > $ mkdir bio
> > $ cd bio
> > ~~~
> > {: .language-bash}
> >
> > Initialise git:
> >
> > ~~~
> > $ git init
> > ~~~
> > {: .language-bash}
> >
> > Create your biography file `me.txt` using `nano` or another text editor.
> > Once in place, add and commit it to the repository:
> >
> > ~~~
> > $ git add me.txt
> > $ git commit -m "Add biography file"
> > ~~~
> > {: .language-bash}
> >
> > Modify the file as described (modify one line, add a fourth line).
> > To display the differences
> > between its updated state and its original state, use `git diff`:
> >
> > ~~~
> > $ git diff me.txt
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

[commit-messages]: https://chris.beams.io/posts/git-commit/
[git-references]: https://git-scm.com/book/en/v2/Git-Internals-Git-References

{% include links.md %}
