# Git Collaboration

## Objectives

1. Make a new branch for your repository with `git branch`.
2. Checkout a branch with `git checkout`.
3. Create and checkout a new branch with `git checkout -b`.
4. Create commits within a branch.
5. Merge branches with `git merge`.
6. Update branches from remotes with `git fetch`.
7. Merge updated remote branches with `git merge`.
8. Update and merge remote branches with `git pull`.

## Overview

Collaborating with git enables teams to work collectively on a project without
interfering with or overwriting each other's work. A key to successful git
collaboration is to keep discrete and individual lines of work isolated until
they are ready to be combined. By following the commands we will be reviewing
in this lab, it is possible for each individual on a team to work on their own,
branched, version of a project while keeping the original version unchanged.

This lab assumes that you have set up git command line functionality in your
terminal. If you have not done this, you can head over to this guide to get
everything you need to follow along with labs on learn.co:

<a href='help.learn.co/workflow-tips/local-environment/mac-osx-manual-environment-set-up'>Mac OSX Manual Environment Set Up</a>

If you type just `git` into your terminal and a list of commands appears, you
are all set and ready to proceed.

Consider the following scenario: You've started work on a big, new feature,
making a few commits that don't entirely finish the feature. Your git log might
look like:

```
512bec5 Still broken, working on new-feature (aviflombaum, 2 hours ago)
62d840 Almost done with new-feature (aviflombaum, 1 day ago)
fbee832 Started new-feature (aviflombaum, 2 days ago)
```

Two days ago we started working on our new-feature. Yesterday we were almost
done. Today we made progress, but it's still broken. In our current state, if
we had to push the repository live and deploy the latest version of our code
to production, our users would see a half-finished, currently broken
new-feature. That's no good.

But no big deal, right? We can just wait until we're done with new-feature to
deploy our code and push the repository live to our users. Here's what
happens though. We notice a big bug that is currently breaking the application
for all users. The bug is an easy fix, one simple change and deploy of your
code can make everything work again. Unfortunately, even if you made that
commit, you can't currently deploy it because while that commit might fix the
bug, you'd still be pushing your half-finished, and broken, new-feature.

```
r4212d1 Fix to application breaking bug (aviflombaum, just now)
512bec5 Still broken, working on new-feature (aviflombaum, 2 hours ago)
62d840 Almost done with new-feature (aviflombaum, 1 day ago)
fbee832 Started new-feature (aviflombaum, 2 days ago)
```

We shouldn't push all those commits. Undoing the work that we've written could
potentially be a complex and frustrating process and might even introduce bugs.
Instead, using git, we can keep our work isolated; we would be able to write a
bug fix to our application and redeploy it while any work we've done on our
new-feature could remain separated until it's completed. We accomplish this
using a feature in git called branches.

## Making a branch with `git branch`

Let's quickly make a repository that we can use as a sandbox to experiment with
the collaborative features of git. You don't have to code along on this lab,
but if you would like to follow along on your terminal for the entire reading,
you can fork and clone a copy of the example repository here:

<a href='https://learn.co/lessons/git-collaboration-readme'>Example Repo</a>

From our home directory we're going to make a new directory for our
mission-critical-application:

```
~ $ mkdir mission-critical-application
~ $ cd mission-critical-application
mission-critical-application $ git init
mission-critical-application $ touch application.rb
mission-critical-application $ git add application.rb
mission-critical-application $ git commit -m "First working version of application.rb"
```

1. We made a new directory with `mkdir mission-critical-application`.
2. We moved into that directory with `cd mission-critical-application`.
3. We turned that directory into a git repository with `git init`.
4. We created our application `touch application.rb`.
5. We programmed an entire working first version in `application.rb` (_not
   reflected in the CLI commands above, but we did, and it was awesome, great
   job_).
6. We added our `application.rb` to git with `git add application.rb`.
7. We committed the first working version of our application with
   `git commit -m "First working version of application.rb"`.
8. You deploy your application to production and people start using it (_also
   not reflected in the CLI commands above, but we did, and it, too, was
   awesome, great job_).

With our application online and customers rolling in, we notice a bug and
quickly add a fix in the form of a file, `first-bug-fix.rb` (_this is just an
example_).

```
mission-critical-application $ touch first-bug-fix.rb
mission-critical-application $ git add first-bug-fix.rb
mission-critical-application $ git commit -m "First bug fix"
```

Right now, our git log could be visualized as a timeline composed of two
commits:

![First Two Commits](https://dl.dropboxusercontent.com/s/ikorf1qvvp4tay0/2015-11-02%20at%2011.15%20AM.png)

### About `master` branch.

Notice that these commits are occurring in a linear sequence of events, almost
like a timeline? We call this timeline a _branch_. Whenever you are working on
commits in git, you are adding them to a branch on that repository's timeline.
The branch you are on by default at the start of any repository, your main
timeline, is called _master_.

![Master Branch](https://dl.dropboxusercontent.com/s/v75as2cf6xr8n8a/2015-11-02%20at%2011.17%20AM.png)

The command, `git status`, will always tell you what branch you are on:

```
mission-critical-application $ git status
On branch master
nothing to commit, working directory clean
```

One of the responsible ways to use git is to make sure that the `master` branch
is always clean with working code so that if we ever need to add a bug fix, we
can add the fix and deploy a new version of the application immediately. We
don't put broken code in `master` so that we can always deploy `master`.

### Starting a new feature with `git branch new-feature`

To keep `master` clean, when we want to start a new feature, we should do it in
an isolated feature branch. Our timeline will look as follows:

![Feature Branch](https://dl.dropboxusercontent.com/s/d61r0fxyriaf5oj/2015-11-02%20at%2011.52%20AM.png)

After commit 2, we will branch out of `master` and create a new timeline for
commits and events specifically related to the new feature. The `master` timeline
remains unchanged and clean. Now that we've covered the idea of the new-feature
branch, let's actually make it.

To make a new branch simply type: `git branch <branch name>`. In the case of a
branch relating to a new feature, we'd name the branch `new-feature` like so:

```
mission-critical-application $ git branch new-feature
```

To see a list of our local branches we can type: `git branch`

```
mission-critical-application $ git branch
* master
  new-feature
```

The `*` in front of the branch `master` indicates that `master` is currently
our working branch and git tells us that we also have a branch called
`new-feature`. If we made a commit right now, that commit would still be
applied to our `master` branch.

### Switching branches with `git checkout`

We need to checkout or move into our `new-feature` timeline or branch so that
git knows that all commits made apply to only that unit of work, timeline, or
branch. We can move between branches with `git checkout <branch name>`.

```
mission-critical-application $ git status
On branch master
nothing to commit, working directory clean
mission-critical-application $ git checkout new-feature
Switched to branch 'new-feature'
mission-critical-application $ git status
On branch new-feature
nothing to commit, working directory clean
```

We started on `master` and then checked out our `new-feature` branch with
`git checkout new-feature`, thereby moving into that timeline.

Let's make a commit in this `new-feature` and get the feature started by making
a new file, `new-feature-file` to represent the code for the new feature.

```
mission-critical-application $ touch new-feature-file
mission-critical-application $ git add new-feature-file
mission-critical-application $ git commit -m "Started new feature"
[new-feature 332a618] Started new feature
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new-feature-file
```

You can see the commit we made was made in the context of the `new-feature`
branch.

Right as we got started on that feature though, we get another bug report and
have to move back into `master` to fix the bug and then deploy `master`. How do we
move from `new-feature` branch back to `master`? What will our code look like
when we move back to `master`, will we see the remnants of the `new-feature`
branch and code represented by the `new-feature-file`?

**Protip: You can create and checkout a new branch in one command using:
`git checkout -b new-branch-name`. That will both create the branch
`new-branch-name` and move into it by checking it out.**

#### Moving back to `master` with `git checkout master`

You can always move between branches with `git checkout`. Since we are
currently on `new-feature`, we can move back to `master` with
`git checkout master`.

```
mission-critical-application $ git checkout master
Switched to branch 'master'
```

And we could move back to `new-feature` with `git checkout new-feature`

```
mission-critical-application $ git checkout new-feature
Switched to branch 'new-feature'
```

And back again with:

```
mission-critical-application $ git checkout master
Switched to branch 'master'
```

![Switching between branches](https://dl.dropboxusercontent.com/s/qzajqsd9f6njauc/2015-11-02%20at%2012.12%20PM.png)

From `master`, one thing you'll notice is that the code you wrote on
`new-feature`, namely the file, `new-feature-file`, is not present in the
current directory.

```
mission-critical-application $ ls
application.rb first-bug-fix.rb
```

The `master` branch only has the code from the most recent commit relative to
the `master` timeline or branch. The code from our `new-feature` is tucked away
in that branch, waiting patiently in isolation from the rest of our code in
`master` for us to finish the feature.

Once you're on `master` you are free to make a commit to fix the bug, which
we'll represent with a new file, `second-bug-fix.rb`.

```
mission-critical-application $ touch second-bug-fix.rb
mission-critical-application $ git add second-bug-fix.rb
mission-critical-application $ git commit -m "Second bug fix"
```

Let's look at our timeline now.

![Commit on Master](https://dl.dropboxusercontent.com/s/9ipgkog7yv8hrok/2015-11-02%20at%2012.18%20PM.png)

We were able to update the timeline in `master` with the bug fix without
touching any of the code in new-feature. The `new-feature` branch and timeline
remains 1 commit behind `master`, because the second bug fix commit occurred in
`master` and `new-feature` branch was created only with the commits from the
moment when the branch was created. You could describe `master` as being 1
commit ahead of the `new-feature` branch.

Let's go back into `new-feature`, complete the feature, commit it and then look
at the timeline. Remember how to move from `master` back to `new-feature`?

```
mission-critical-application $ git status
On branch master
nothing to commit, working directory clean
mission-critical-application $ git checkout new-feature
Switched to branch 'new-feature'
```

Let's rename `new-feature-file` to `new-feature` to signify the code we wrote
to complete the feature and commit this change. We can rename a file with `mv <original filename> <new filename>` bash command.

```
mission-critical-application $ mv new-feature-file new-feature
mission-critical-application $ git add new-feature
mission-critical-application $ git commit -m "Finished feature"
[new-feature bfe50fc] Finished feature
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new-feature
```

Let's see our timeline now:

![Completed Feature Branch](https://dl.dropboxusercontent.com/s/xtoehu7tv5zim6v/2015-11-02%20at%2012.31%20PM.png)

The final step of our `new-feature` work sprint is to figure out how to merge
that timeline into the `master` timeline.

## Merging branches with `git merge`

Our goal is to bring the timeline of commits that occurred on the `new-feature`
branch into the `master` so that at the end of the operation, our `master`
timeline looks like:

![Merged Timeline](https://dl.dropboxusercontent.com/s/bf0cktf3ag549z2/2015-11-02%20at%201.15%20PM.png)

By merging the timelines, `master` will have all of the commits from the
`new-feature` branch as though those events occurred on the `master` timeline.

When we merge a branch with `git merge`, it's important to be currently working
on your target branch, the branch you want to move into. The first step for our
`new-feature` merge is to checkout `master` because that is where we want the
commits to end up.

```
mission-critical-application $ git checkout master
Switched to branch 'master'
```

Once you are on your target branch, type: `git merge <branch name>` where
`<branch name>` is the branch we want to merge. In this case,
`git merge new-feature` will do the trick.

```
mission-critical-application $ git merge new-feature
Merge made by the 'recursive' strategy.
 new-feature      | 0
 new-feature-file | 0
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new-feature
 create mode 100644 new-feature-file
```

Now the branches have been merged to `master`, if you type `ls` into your
terminal, you'll see the `new-feature` file from the `new-feature` branch in
your current working directory. Notice, though, that you should also have a
`new-feature-file` present. If you go back and take a look at the `master`
timeline image, you can see why: We added the `new-feature-file` with commit 3,
'Started feature,' changed the file name to `new-feature` with commit 5,
'Finished feature,' but when the two commits were merged with `master`,
git considered these as two separate files due to the name change, and added
both.

Had we created our initial `new-feature-file` in commit 3, then modified
the contents _without changing the file name_ in commit 5, only
`new-feature-file` would be present, and the changes made during the 5th commit
would appear in our current `master` branch.

## Pushing your work to a remote repository with `git push`

Let's say we would like to share our awesome new application with our team, so
they too can clone down the files, create their own feature branches, and make
this app even better. We do this using `git push`.

The `git push` command sends the current version of the branch you are on to a
remote repository. If you have cloned down a repository from GitHub, using
`git push` will attempt to push your local work to the remote repository and
merge your work with the remote timeline, along with all of your commits.

In the example below, `git add .` will 'stage' _all_ modified files in your
local repository, setting them up so that you can commit your recent changes.

```
mission-critical-application $ git add .
mission-critical-application $ git commit -m 'fixed application bugs'
mission-critical-application $ git push
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 268 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
...
```

In a collaborative project, if there are no conflicts, using just the above
commands would push your local work to the shared remote, and set it as the
most up-to-date master. Remember, if we want to adhere to good practices, only
clean, working code should be pushed to the `master` branch. Be aware: pushing
to `master` on a team project can potentially overwrite the work of others,
delete files or even add previously deleted files back in.

One way to help prevent this is to `git push` from a local branch instead of
your `master` branch. Checkout a new branch called `second-new-feature`, make
any change to your files, and type `git push`. You will likely see the
following:

```
fatal: The current branch second-new-feature has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin second-new-feature
```

If you see this, it is because there is no record of your new local branch on
the remote repository. Copy the provided command, run it in your terminal, and
your newly created branch will be available online. Pushing up a branch will
allow others to access your branch's code without interfering with the remote
`master` branch.

In order to _get_ data from a remote `master` branch, we will use
`git fetch` and `git pull`.

## Accessing remote branches with `git fetch` and `git pull`

Whenever you want to update your local versions with their related remote
branches, you can use `git fetch`:

```
mission-critical-application $ git fetch
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 4 (delta 3), reused 3 (delta 2), pack-reused 0
Unpacking objects: 100% (4/4), done.
From github.com:aviflombaum/mission-critical-application
   bfe50fc..0ae1da2  master     -> origin/master
```

The `git fetch` command will retrieve any updated data for all local branches
that have a remote counterpart, regardless of which branch you are currently in.
The first line, `From github.com:aviflombaum/mission-critical-application` is
informing us which remote our `git fetch` updated from, namely, the remote
repository located at: https://github.com/aviflombaum/mission-critical-application

When we `fetch` with git, we are asking to copy all changes on the remote to
our local git repository, but not actually integrate any of those changes. The
next line, `bfe50fc..0ae1da2 master -> origin/master` is telling us that a new
commit was found in `origin/master`. `origin/master` means the GitHub version
of `master`. Even though git fetched a new commit from `origin/master`, it did
not merge it into the local `master`.

![Fetch without integration](https://dl.dropboxusercontent.com/s/iy2jovft8ykrxbd/2015-11-02%20at%202.08%20PM.png)

After you fetch, you have access to the remote code but you still have to merge
it. From within your local `master` branch, type `git merge origin/master`,
referring to the branch's full path.

```
mission-critical-application $ git merge origin/master
Updating bfe50fc..0ae1da2
Fast-forward
 remote-bug-fix | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 remote-bug-fix
mission-critical-application $ ls
 application.rb		new-feature-file    new-feature
 remote-bug-fix
```

The commits fetched via `git fetch` are now merged from the `origin/master`
branch into our local `master` branch. And now `ls` reveals that the file
present on the remote, `remote-bug-fix` is integrated into our local copy of
`master` as well.

_Note:_ The `git merge` command will merge whatever branch you're currently in
with the branch you specify. If you have checked out a local branch and
have used `git fetch` to get updated code, using`git merge <local branch name>`
will merge the local and remote versions of that branch. If you use
`git merge <local branch name>` while you're on your `master`, your master will
merge data from the fetched branch!

If we want to access a remote branch we do not have locally, we first want to
check for the name of all remote branches using `git branch -a`:

```
mission-critical-application $ git branch -a
* master
  new-feature
  second-new-feature
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/remote-feature-branch
```

We can see here that there is a branch present, `remote-feature-branch` that we
do not have locally. We can use `git fetch` again to retrieve that branch, but
this time, we will specify what we want using the name of the remote branch,
and then use `git checkout` to switch over:

```
mission-critical-application $ git branch
* master
mission-critical-application $ git fetch origin remote-feature-branch
From github.com:aviflombaum/mission-critical-application
 * branch            remote-feature-branch -> FETCH_HEAD
mission-critical-application $ git checkout remote-feature-branch
Branch remote-feature-branch set up to track remote branch remote-feature-branch from origin.
Switched to a new branch 'remote-feature-branch'
mission-critical-application $ git branch
  master
  new-feature
* remote-feature-branch
  second-new-feature
```

When we checkout a fetched remote branch, git will create a local branch to
track that remote and switch to that branch. We can now do work on that branch,
push it back up to GitHub, and another developer can fetch those changes down.

We don't often use the `git fetch` command because it always requires two
steps: first, `git fetch`, and second, `git merge`, to actually integrate those
changes into your working branch. Generally, if you are in `master` you want to
immediately `fetch` and `merge` any changes to the remote `master`.

### Combining `git fetch` with `git merge` by using `git pull`

If you want to both fetch and merge, which is what you want to do 99% of the
time, just type `git pull`. The `git pull` command is literally the combination
of both `git fetch` and `git merge`.

When you `git pull` the following things will occur:

1. You will `git fetch` all remote changes for your current branch.
2. Any changes that are on a remote branch will be automatically merged with
   your local branch.

## Conclusion

On a collaborative project, you will likely use these commands in various
ways. Sometimes, you might be working with a peer on the same branch of code,
and will need to share, pushing and pulling, any updates as you build your
application. Other times, peers may need you to push up your `master` so that
they can start new feature branches based on your work.

Git is complex, and collaborating with people in this matter is just hard -
there's no easy way to allow hundreds of people to all work on the same code
base. These workflows are just being introduced to you and you'll have lots of
time to practice them and memorize what each command does. Don't try to learn
it all at once; instead just start to get an understanding of what's what. As
a best practice in general, make sure to `git add .` and `git commit` any
notable changes to your code. There are ways to revert your code back to these
commits, even in the event that your `master` is overwritten.

![XKCD Git](http://imgs.xkcd.com/comics/git.png)

Resources:

* <a href='https://services.github.com/on-demand/resources/cheatsheets/'>GitHub Cheatsheets</a>

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/git-collaboration-readme'>Git Collaboration</a> on Learn.co and start learning to code for free.</p>
