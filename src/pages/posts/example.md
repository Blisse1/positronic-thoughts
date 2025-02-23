---
layout: ../../layouts/MarkdownPostLayout.astro
title: "Git Notes from ProGit"
pubDate: 2025-02-23
description: "Git Notes"
author: "Bliss"
tags: ["git"]
---

## Git notes
## About git add
"If you modify a file after you run git add, you have to run git add again to stage the
latest version of the file"
## About git status
New files that aren’t tracked have a ?? next to them
New files that have been added to the staging area have an A
Modified files have an M and so on
There are two columns to the output — the lefthand column indicates the status
of the staging area and the right-hand column indicates the status
of the working tree
## About git diff
Git diff compares what is in your working directory with what is on your staging area.
The results tells you the changes you've made that you haven't staged.

If you want to see what you've staged that will go to your next commit, you can use ```git
diff --staged```

It’s important to note that git diff by itself doesn’t show all changes made since your last
commit — only changes that are still unstaged. If you’ve staged all of your changes, git diff will
give you no output.
## About git commit
For an even more explicit reminder of what you’ve modified, you can pass the -v
option to git commit. Doing so also puts the diff of your change in the editor so you
can see exactly what changes you’re committing
Adding the -a option to the git commit command makes
Git automatically stage every file that is already tracked before doing the commit, letting you skip
the git add part: git commit -a -m "Add new benchmark"
## About git rm
If you modified the file or
had already added it to the staging area, you must force the removal with the -f option.
Another useful thing you may want to do is to keep the file in your working tree but remove it from
your staging area. This is particularly useful if you forgot to add something to your .gitignore
file and accidentally staged it, like a large log file or a bunch of .a compiled files. To do this, use the
--cached option: git rm --cached FILE
## About moving files on git
git mv file_from file_to
In fact, if you run something like this and look at the status, you’ll see that Git
considers it a renamed file: git mv file_from file_to
The only real difference is that git mv is one command instead of three
## About git log
One of the more helpful options is -p or --patch, which shows the difference (the patch output)
introduced in each commit. You can also limit the number of log entries displayed, such as using -2
to show only the last two entries.
Another really useful option is --pretty. This option changes the log output to formats other than
the default. A few prebuilt option values are available for you to use. The oneline value for this
option prints each commit on a single line, which is useful if you’re looking at a lot of commits. In
addition, the short, full, and fuller values show the output in roughly the same format but with
less or more information, respectively
git log --pretty=format:"%h - %an, %ar : %s"
This option adds a nice little ASCII graph showing your branch and merge history:
git log --pretty=format:"%h %s" --graph
However, the time-limiting options such as --since and --until are very useful. For example, this
command gets the list of commits made in the last two weeks
git log --since=2.weeks
For instance, if you wanted to find the last commit that added or removed a reference to a
specific function, you could call: git log -S function_name
The last really useful option to pass to git log as a filter is a path. If you specify a directory or file
name, you can limit the log output to commits that introduced a change to those files
git log -- path/to/file
## About undoing things (ammend)
One of the common undos takes place when you commit too early and possibly forget to add some
files, or you mess up your commit message. If you want to redo that commit, make the additional
changes you forgot, stage them, and commit again using the --amend option: $ git commit --amend
It’s important to understand that when you’re amending your last commit, you’re
not so much fixing it as replacing it entirely with a new, improved commit that
pushes the old commit out of the way and puts the new commit in its place.
Effectively, it’s as if the previous commit never happened, and it won’t show up in
your repository history.
The obvious value to amending commits is to make minor improvements to your
last commit, without cluttering your repository history with commit messages of
the form, “Oops, forgot to add a file” or “Darn, fixing a typo in last commit”.
Only amend commits that are still local and have not been pushed somewhere.
Amending previously pushed commits and force pushing the branch will cause
problems for your collaborators
## About Unmodifying a modified file
file? How can
you easily unmodify it — revert it back to what it looked like when you last committed (or initially
cloned, or however you got it into your working directory)?
Using git restore.
Remember, anything that is committed in Git can almost always be recovered. Even commits that
were on branches that were deleted or commits that were overwritten with an --amend commit can
be recovered (see Data Recovery for data recovery). However, anything you lose that was never
committed is likely never to be seen again.
It’s important to understand that git restore <file> is a dangerous command
Don’t ever use this command unless you
absolutely know that you don’t want those unsaved local changes
## About git merge
La rama main debe tener como minimo 2 commits para poder realizar un merge
con otra rama y que pueda generar un conflicto que obligue a decidir
qué contenido quiere mantener de la rama main o de la rama que se quiere mergear
De lo contrario, si solo tiene 1 commit la rama main y se trata de mergear
con otra rama, main estará apuntando al contenido del commit de la rama que se quiso mergear
Uno se puede devolver a que main apunte al commit inicial usando un git reset --hard
## About git branches
How does Git know what branch you’re currently on? It keeps a special pointer called HEAD.
The git branch command only created a new branch — it didn’t switch to that branch
You can easily see this by running a simple git log command that shows you where the branch
pointers are pointing. This option is called --decorate. git log --oneline --decorate
To switch to an existing branch, you run the git checkout command. This moves HEAD to point to the testing branch.
## About switching branches
Switching branches changes files in your working directory
It’s important to note that when you switch branches in Git, files in your working
directory will change. If you switch to an older branch, your working directory
will be reverted to look like it did the last time you committed on that branch. If Git
cannot do it cleanly, it will not let you switch at all.
## About creating branches
From Git version 2.23 onwards you can use git switch instead of git checkout to:
• Switch to an existing branch: git switch testing-branch.
• Create a new branch and switch to it: git switch -c new-branch. The -c flag
stands for create, you can also use the full flag: --create.
• Return to your previously checked out branch: git switch -
## About git log 2
git log doesn’t show all the branches all the time
If you were to run git log right now, you might wonder where the "testing"
branch you just created went, as it would not appear in the output.
The branch hasn’t disappeared; Git just doesn’t know that you’re interested in that
branch and it is trying to show you what it thinks you’re interested in. In other
words, by default, git log will only show commit history below the branch you’ve
checked out.
To show commit history for the desired branch you have to explicitly specify it: git
log testing. To show all of the branches, add --all to your git log command.
## About git branching and merging
It’s best to have a clean working state when you switch branches.
There are ways to get around this (namely, stashing and commit amending) that we’ll cover later on, in Stashing and Cleaning.
To phrase that another way, when you try to merge one commit with a commit
that can be reached by following the first commit’s history, Git simplifies things by moving the
pointer forward because there is no divergent work to merge together — this is called a “fastforward.”
It’s worth noting here that the work you did in your hotfix branch is not contained in the files in
your iss53 branch. If you need to pull it in, you can merge your master branch into your iss53
branch by running git merge master, or you can wait to integrate those changes until you decide to
pull the iss53 branch back into master later.
This looks a bit different than the hotfix merge you did earlier. In this case, your development
history has diverged from some older point. Because the commit on the branch you’re on isn’t a
direct ancestor of the branch you’re merging in, Git has to do some work. In this case, Git does a
simple three-way merge, using the two snapshots pointed to by the branch tips and the common
ancestor of the two.
Instead of just moving the branch pointer forward, Git creates a new snapshot that results from this
three-way merge and automatically creates a new commit that points to it. This is referred to as a
merge commit, and is special in that it has more than one parent.
## About basic merge conflicts
If you changed the same part of the same file
differently in the two branches you’re merging, Git won’t be able to merge them cleanly
Git hasn’t automatically created a new merge commit. It has paused the process while you resolve
the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can
run git status
## Branch management
Do not rename branches that are still in use by other collaborators. Do not rename
a branch like master/main/mainline without having read the section Changing the
master branch name.
## Branching Workflows
Many Git developers have a workflow that embraces this approach, such as having only code that is
entirely stable in their master branch — possibly only code that has been or will be released. They
have another parallel branch named develop or next that they work from or use to test stability — it
isn’t necessarily always stable, but whenever it gets to a stable state, it can be merged into master.
It’s used to pull in topic branches (short-lived branches, like your earlier iss53 branch) when
they’re ready, to make sure they pass all the tests and don’t introduce bugs.
## Remote Branches

## Using HTTPS
Don’t type your password every time
If you’re using an HTTPS URL to push over, the Git server will ask you for your
username and password for authentication. By default it will prompt you on the
terminal for this information so the server can tell if you’re allowed to push.
If you don’t want to type it every single time you push, you can set up a “credential
cache”. The simplest is just to keep it in memory for a few minutes, which you can
easily set up by running git config --global credential.helper cache.
## Tracking Branches
Checking out a local branch from a remote-tracking branch automatically creates what is called a
92
“tracking branch” (and the branch it tracks is called an “upstream branch”). Tracking branches are
local branches that have a direct relationship to a remote branch. If you’re on a tracking branch
and type git pull, Git automatically knows which server to fetch from and which branch to merge
in.
If the branch name you’re trying to checkout (a) doesn’t exist and (b) exactly matches a name on only one remote, Git will
create a tracking branch for you:
```bash
$ git checkout serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```
If you already have a local branch and want to set it to a remote branch you just pulled down, or
want to change the upstream branch you’re tracking, you can use the -u or --set-upstream-to
option to git branch to explicitly set it at any time
```bash
git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```
Generally it’s better to simply use the fetch and merge commands explicitly as the magic of git pull
can often be confusing.
## Rebasing
In Git, there are two main ways to integrate changes from one branch into another: the merge and
the rebase.
With the rebase command, you can take all the
changes that were committed on one branch and replay them on a different branch.

