As we saw in the previous episode, we can refer to commits by their identifiers.
You can refer to the _most recent commit_ of the working branch by using the
identifier `HEAD`.
To illustrate, let's make a change to `index.md`.
$ nano index.md
$ cat index.md
# Seth Erickson

I am a data services librarian at UCSB.

My responsibilities include:

- Teaching Carpentry Workshops
- Helping students learn Git

See my [reading list](reading-list.html).

Contact: <my email>
$ git diff HEAD index.md
diff --git a/index.md b/index.md
index d9f35ba..f9d4112 100644
--- a/index.md
+++ b/index.md
@@ -8,3 +8,5 @@ My responsibilities include:
 - Helping students learn Git
 
 See my [reading list](reading-list.html).
+
+Contact: serickson@ucsb.edu
real goodness in all this is when you can refer to previous commits.  We do that
by adding `~1` (where "~" is "tilde", pronounced [**til**-d*uh*]) to refer to
the commit one before `HEAD`.
$ git diff HEAD~1 index.md
$ git diff HEAD~2 index.md
diff --git a/index.md b/index.md
index 93d5098..f9d4112 100644
--- a/index.md
+++ b/index.md
@@ -7,3 +7,6 @@ My responsibilities include:
 - Teaching Carpentry Workshops
 - Helping students learn Git
 
+See my [reading list](reading-list.html).
+
+Contact: serickson@ucsb.edu
$ git show HEAD~2 index.md
commit d8dd09e46f76a9e8560d57bd31ba99059eb27bad
Author: Seth Erickson <sr.erickson@gmail.com>
Date:   Mon Apr 24 11:19:41 2023 -0700
    fixing a typo (commit #3)
diff --git a/index.md b/index.md
index c826feb..93d5098 100644
--- a/index.md
+++ b/index.md
@@ -1,6 +1,6 @@
 # Seth Erickson
 
-I am a data services librarian at UCSB
+I am a data services librarian at UCSB.
 
 My responsibilities include:
In this way, we can build up a chain of commits. The most recent end of the
chain is referred to as `HEAD`; we can refer to previous commits using the `~`
notation, so `HEAD~1` means "the previous commit", while `HEAD~123` goes back
123 commits from where we are now.
We can also refer to commits using those long strings of digits and letters that
`git log` displays. These are unique IDs for the changes, and "unique" really
does mean unique: every change to any set of files on any computer has a unique
40-character identifier. Our `HEAD~2` comment addition was given the ID
`7f9e4987af8d390eac5205e9a760e3f72204041c`, so let's try this:
$ git diff d8dd09e46f76a9e8560d57bd31ba99059eb27bad index.md
diff --git a/index.md b/index.md
index 93d5098..f9d4112 100644
--- a/index.md
+++ b/index.md
@@ -7,3 +7,6 @@ My responsibilities include:
 - Teaching Carpentry Workshops
 - Helping students learn Git
 
+See my [reading list](reading-list.html).
+
+Contact: serickson@ucsb.edu
That's the right answer, but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters (typically seven for normal
size projects):
$ git diff d8dd09e index.md
diff --git a/index.md b/index.md
index 93d5098..f9d4112 100644
--- a/index.md
+++ b/index.md
@@ -7,3 +7,6 @@ My responsibilities include:
 - Teaching Carpentry Workshops
 - Helping students learn Git
 
+See my [reading list](reading-list.html).
+
+Contact: serickson@ucsb.edu
All right! So we can save changes to files and see what we've changed. Now, how
can we restore older versions of things? Let's suppose we change our mind about
the last update to `index.md` (the "ill-considered change").
    modified:   index.md
$ git checkout HEAD index.md
$ cat index.md
# Seth Erickson

I am a data services librarian at UCSB.

My responsibilities include:

- Teaching Carpentry Workshops
- Helping students learn Git

See my [reading list](reading-list.html).
As you might guess from its name, `git checkout` checks out (i.e., restores) an
old version of a file. In this case, we're telling Git that we want to recover
the version of the file recorded in `HEAD`, which is the last saved commit. If
we want to go back even further, we can use a commit identifier instead:
$ git checkout f14106 index.md
$ cat index.md
    modified:   index.md
$ git checkout HEAD index.md
> $ git checkout f14106 index.md
> to revert `index.md` to its state after the commit `f14106`. But be careful!
> if you forget `index.md` in the previous command.
> $ git checkout f14106
> The "detached HEAD" is like "look, but don't touch" here, so you shouldn't
> make any changes in this state. After investigating your repo's past state,
> reattach your `HEAD` with `git checkout main`. 
It's important to remember that we must use the commit number that identifies
the state of the repository *before* the change we're trying to undo. A common
mistake is to use the number of the commit in which we made the change we're
trying to discard. In the example below, we want to retrieve the state from
before the most recent commit (`HEAD~1`), which is commit `94456da`:
> As it says, `git checkout` without a version identifier restores files to the
> state saved in `HEAD`. The double dash `--` is needed to separate the names of
> the files being recovered from the command itself: without it, Git would try
> to use the name of the file as the commit identifier. 
The fact that files can be reverted one by one tends to change the way people
organize their work. If everything is in one large document, it's hard (but not
impossible) to undo changes to the introduction without also undoing changes
made later to the conclusion. If the introduction and conclusion are stored in
separate files, on the other hand, moving backward and forward in time becomes
much easier.
> Consider this command: `git diff HEAD~9 index.md`. What do you predict this command
> Try another command, `git diff [ID] index.md`, where [ID] is replaced with
> Make a change to `index.md`, add that change, and use `git checkout` to see if
> You would like to find a commit that modifies some specific text in `index.md`.
> e.g., `git diff index.md`. We can apply a similar idea here.
> $ git log index.md
> $ git log --patch index.md