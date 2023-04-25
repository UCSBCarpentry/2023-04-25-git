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

## Adding files


Let's create a text file called `index.md` and add it to our repository. The
file will later be converted into a webpage by GitHub pages. We'll write the
file using a syntax called Markdown, which is why we use the `.md` extensions.

> ## Markdown
> [Markdown](https://www.markdownguide.org/) is a language used to simplify writing HTML. Plain text characters
> like `#` and `*` are used in place of HTML tags. These characters are then
> processed (by GitHub pages) and transformed into HTML tags. As the name
> Markdown suggests, the language has been trimmed down to a minimum. The most
> frequently used elements, like headings, paragraphs, lists, tables and basic
> text formatting (i.e. bold, italic) are part of Markdown. Markdown’s
> simplified syntax keeps content human-readable.
{: .callout}

We'll use `nano` to edit the file; you can use whatever editor you like. In
particular, this does not have to be the `core.editor` you set globally earlier.
But remember, the bash command to create or edit a new file will depend on the
editor you choose (it might not be `nano`). For a refresher on text editors,
check out ["Which
Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) in [The Unix
Shell](https://swcarpentry.github.io/shell-novice/) lesson.


~~~
$ nano index.md
~~~
{: .language-bash}

Type a few lines about yourself. (Use the `#` to create a section header).

~~~
# Seth Erickson

I am a data services librarian at UCSB
~~~
{: .output}

Let's first verify that the file was properly created by running the list command (`ls`):

~~~
$ ls
~~~
{: .language-bash}

~~~
index.md
~~~
{: .output}


`index.md` is a text file, which we can see by running:

~~~
$ cat index.md
~~~
{: .language-bash}

~~~
# Seth Erickson

I am a data services librarian at UCSB
~~~
{: .output}

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
	index.md

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

~~~
$ git add index.md
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
	new file:   index.md
~~~
{: .output}

Git now knows that it's supposed to keep track of `index.md`,
but it hasn't recorded these changes as a commit yet.
To get it to do that,
we need to run one more command:

~~~
$ git commit -m "adding initial page content (commit #1)"
~~~
{: .language-bash}

~~~
[main (root-commit) f14106c] adding initial page content (commit #1)
 1 file changed, 3 insertions(+)
 create mode 100644 index.md
~~~
{: .output}

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [commit]({{ page.root }}{% link reference.md %}#commit)
(or [revision]({{ page.root }}{% link reference.md %}#revision)) and its short identifier is `f14106c`. Your commit may have another identifier.

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
commit f14106cb34f6983fa329168c97042739e4deec77 (HEAD -> main)
Author: Seth Erickson <xxx@yyy.com>
Date:   Mon Apr 24 11:01:07 2023 -0700

    adding initial page content (commit #1)
~~~
{: .output}

`git log` lists all commits  made to a repository in reverse chronological
order. The listing for each commit includes the commit's full identifier (which
starts with the same characters as the short identifier printed by the `git
commit` command earlier), the commit's author, when it was created, and the log
message Git was given when the commit was created.

> ## Where Are My Changes?
>
> If we run `ls` at this point, we will still see just one file called `index.md`.
> That's because Git saves information about files' history
> in the special `.git` directory mentioned earlier
> so that our filesystem doesn't become cluttered
> (and so that we can't accidentally edit or delete an old version).
{: .callout}

Now let's add more to `index.md`. (Again, we'll edit with `nano` and then `cat`
the file to show its contents; you may use a different editor, and don't need to
`cat`.)

~~~
$ nano index.md
~~~
{: .language-bash}

Let's add a list of job responsibilities. (In Markdown, a line beginning with a
`-` and space is converted into an HTML list with bullet points.)

~~~
# Seth Erickson

I am a data services librarian at UCSB

My responsibilities include:

- Teaching Carpentry Workshops
- Helping students learn Git
~~~
{: .output}

When we run `git status` now, it tells us that a file it already knows about has
been modified:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   index.md

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

The last line is the key phrase: "no changes added to commit". We have changed
this file, but we haven't told Git we will want to save those changes (which we
do with `git add`) nor have we saved them (which we do with `git commit`). So
let's do that now. It is good practice to always review our changes before
saving them. We do this using `git diff`. This shows us the differences between
the current state of the file and the most recently saved version:

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/index.md b/index.md
index 9b284ec..c826feb 100644
--- a/index.md
+++ b/index.md
@@ -1,3 +1,9 @@
 # Seth Erickson
 
 I am a data services librarian at UCSB
+
+My responsibilities include:
+
+- Teaching Carpentry Workshops
+- Helping students learn Git
+
~~~
{: .output}

The output is cryptic because it is actually a series of commands for tools like
editors and `patch` telling them how to reconstruct one file given the other. If
we break it down into pieces:

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
$ git commit -m "add list of responsibilities (commit #2)"
~~~
{: .language-bash}

~~~
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   index.md

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Whoops: Git won't commit because we didn't use `git add` first. Let's fix that:

~~~
$ git add index.md
$ git commit -m "add list of responsibilities (commit #2)"
~~~
{: .language-bash}

~~~
[main bc5ac3e] add list of responsibilities (commit #2)
 1 file changed, 6 insertions(+)
~~~
{: .output}

Git insists that we add files to the set we want to commit before actually
committing anything. This allows us to commit our changes in stages and capture
changes in logical portions rather than only large batches. For example, suppose
we're adding a few citations to relevant research to our thesis. We might want
to commit those additions, and the corresponding bibliography entries, but *not*
commit some of our work drafting the conclusion (which we haven't finished yet).

To allow for this, Git has a special *staging area* where it keeps track of
things that have been added to the current [changeset]({{ page.root }}{% link
reference.md %}#changeset) but not yet committed.

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

Let's watch as our changes to a file move from our editor to the staging area
and into long-term storage. First, let's make another change: I'll fix a typo by
adding a period to the bio line.

~~~
$ nano index.md
~~~
{: .language-bash}


~~~
# Seth Erickson

I am a data services librarian at UCSB.

My responsibilities include:

- Teaching Carpentry Workshops
- Helping students learn Git

~~~
{: .output}

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/index.md b/index.md
index c826feb..93d5098 100644
--- a/index.md
+++ b/index.md
@@ -1,6 +1,6 @@
 # Seth Erickson
 
-I am a data services librarian at UCSB
+I am a data services librarian at UCSB.
 
 My responsibilities include:
~~~
{: .output}

So far, so good. Now let's put that change in the staging area and see what `git
diff` reports:

~~~
$ git add index.md
$ git diff
~~~
{: .language-bash}

There is no output: as far as Git can tell, there's no difference between what
it's been asked to save permanently and what's currently in the directory.
However, if we do this:

~~~
$ git diff --staged
~~~
{: .language-bash}

~~~
diff --git a/index.md b/index.md
index c826feb..93d5098 100644
--- a/index.md
+++ b/index.md
@@ -1,6 +1,6 @@
 # Seth Erickson
 
-I am a data services librarian at UCSB
+I am a data services librarian at UCSB.
 
 My responsibilities include:
~~~
{: .output}

it shows us the difference between the last committed change and what's in the
staging area. Let's save our changes:

~~~
$ git commit -m "fixing a typo (commit #3)"
~~~
{: .language-bash}

~~~
[main d8dd09e] fixing a typo (commit #3)
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
commit d8dd09e46f76a9e8560d57bd31ba99059eb27bad (HEAD -> main)
Author: Seth Erickson <...>
Date:   Mon Apr 24 11:19:41 2023 -0700

    fixing a typo (commit #3)

commit bc5ac3ec2412f7e4bfb4da2d7085d3df4cc514ea
Author: Seth Erickson <...>
Date:   Mon Apr 24 11:11:40 2023 -0700

    add list of responsibilities (commit #2)

commit f14106cb34f6983fa329168c97042739e4deec77
Author: Seth Erickson <...>
Date:   Mon Apr 24 11:01:07 2023 -0700

    adding initial page content (commit #1)
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
> commit d8dd09e46f76a9e8560d57bd31ba99059eb27bad (HEAD -> main)
> Author: Seth Erickson <...>
> Date:   Mon Apr 24 11:19:41 2023 -0700
> 
>     fixing a typo (commit #3)
> (END)
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
> d8dd09e (HEAD -> main) fixing a typo (commit #3)
> bc5ac3e add list of responsibilities (commit #2)
> f14106c adding initial page content (commit #1)
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
> * d8dd09e (HEAD -> main) fixing a typo (commit #3)
> * bc5ac3e add list of responsibilities (commit #2)
> * f14106c adding initial page content (commit #1)
> ~~~
> {: .output}
{: .callout}


## Directories in Git
Two important facts you should know about directories in Git:

1.   Git does not track directories on their own, only files within them.
2.   If you can create a directory in your Git repository and populate it with files,
     you can add all the files in the directory at once.  

Create a new directory named `images`
We will be using this directory in the following lessons so please follow along.

~~~
$ mkdir images
$ git status
$ git add images
$ git status
~~~
{: .language-bash}

Note, our newly created empty directory `images` does not appear in
the list of untracked files even if we explicitly add it (_via_ `git add`) to our
repository. This is the reason why you will sometimes see `.gitkeep` files
in otherwise empty directories. Unlike `.gitignore`, these files are not special
and their sole purpose is to populate a directory so that Git adds it to
the repository. In fact, you can name such files anything you like.

Try adding an empty file in our new directory:

~~~
$ touch images/.gitkeep
$ git status
$ git add images
$ git status
~~~
{: .language-bash}

Before moving on, we will commit these changes.

~~~
$ git commit -m "Add images folder"
~~~
{: .language-bash}

To recap, when we want to add changes to our repository, we first need to add
the changed files to the staging area (`git add`) and then commit the staged
changes to the repository (`git commit`):

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

The staging area can hold changes from any number of files that you want to
commit as a single snapshot. The goals for the rest of this lesson are:

1. Add some text to `index.md`
2. Create a new file `reading-list.md` with a list of books you want to read
3. Add changes from both files to the staging area, and commit those changes.

You can add both files to the staging area. We can do that in one line:

~~~
$ git add index.md reading-list.md
~~~
{: .language-bash}
Or with multiple commands:
~~~
$ git add index.md
$ git add reading-list.md
~~~
{: .language-bash}
Now the files are ready to commit. You can check that using `git status`. If you are ready to commit use:
~~~
$ git commit -m "update page and add reading list"
~~~
{: .language-bash}
~~~
[main 86abfdb] add reading list
 2 files changed, 2 insertions(+)
 create mode 100644 reading-list.md
~~~

[commit-messages]: https://chris.beams.io/posts/git-commit/
[git-references]: https://git-scm.com/book/en/v2/Git-Internals-Git-References

{% include links.md %}
