##Git Collaboration Basics

###Objectives
1. Describe how branching allows for collaboration
2. Define the master branch
3. Create and switch to new branches using `git branch` and `git checkout`
4. Merge branches using `git merge`
5. Delete local branches using `git branch -d`
6. Distinguish between and use `git fetch` and `git pull` to get files from remote repositories


###Overview
This lesson is an intro to basic git collaboration flow.

###Intro
Branching allows for collaboration because when you are working on your own features, you don't necessarily want everyone to see what you're doing while it's still being created. But! you want the features of Git. You want to be able to commit, move back, move forward and tell the story of developing the feature. So we have branches. Branches allow you to take a snapshot of your code from master and work on it separately from the actual master branch. Once you're done, you merge it back into master. If anyone of this is confusing don't worry, we will break it down piece by piece.

###Master Branch
What do we mean when we say "master branch"? The master branch should be a our main pristine branch. Any changes you plan on making should never be done on master, always on a separate branch.

###`git branch`
Let's say we're in a brand new directory called `twitter_for_dogs`. After we `cd` into it and run `git init` to set up tracking, we can then start building our app. Before we do anything, let's run `git branch` in our terminal. This command will print out all of our branches as well as show us which branch we are on. Since we are in a brand new project, we only have a `master` branch. Your output will look like this.

```bash
* master
```
This is telling us we have one branch, our default `master` branch, which we are currently on indicated by a `*`.

Since we are creating an awesome new app, we probably want to start coding. Now is the time to create a new branch to start our work on. We can do this by checking out a new branch, which we will discuss now.

###`git checkout`
If we type `git checkout` in our terminal, by default we will stay on our current `master` branch. In order to create a new branch, we have to pass it the `-b`, for branch, flag. We also have to pass it a name for the new branch. If we type `git checkout -b new_feature`, now we will create a new branch called `new_feature`. Git will also switch us to that new branch.

The output from this command will be:

```bash
Switched to a new branch 'new_feature'
```
This let's you know that you've created a new branch and you are now on it.


Now if we type `git branch` again in our terminal it will say:

```bash
master
* new_feature
```
Here we can see we have two branches, `master` and `new_feature` and we are now on our brand new `new_feature` branch.

***Note*** If for some reason you've already created a `new_feature` branch the output would be:

```bash
fatal: A branch named 'new_feature' already exists.
```

###`git merge`
Ok so we have built out our new feature on our `new_feature` branch, added and commited the changes, and we want to merge those changes back into our `master` branch. We can do this with `git merge`. When merging, you have to be on the branch you want to merge into. So in our case, we want to be on `master`. When on master we can type `git merge new_feature` to add the changes to our `master` branch.

###`git branch -d`
Now that we've merged our `new_feature` branch, we don't really need it anymore, so let's delete it. You can do this with `git branch -d new_feature`. This command takes a flag `-d`, which stands for delete, then it takes the name of the branch.

***Note*** You cannot delete a branch that you are current on. For example, if you are on the `new_feature` branch and you try and delete it the output will be:

```bash
error: Cannot delete the branch 'new_feature' which you are currently on.
```
If you see this message you can switch to `master` and run the command again. You will then see something like this:

```bash
Deleted branch new_feature (was 2528449).
```
If we run `git branch` again you will see that we are back to just one branch, `master`.
