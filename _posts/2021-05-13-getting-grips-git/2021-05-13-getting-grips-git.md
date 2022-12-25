---
layout: post
title: "Getting To Grips With GIT"
date: "2021-05-13"
description: ""
coverimage: Getting_To_Grips_With_GIT_eg7dD1v.jpg
tags: git
published: true
posttype: article
categories: tutorial
---
## Creating a Git development branch

You can list all of your current branches like this:
```
git branch -a
```
This shows all of the local and remote branches. Assuming you only have a single master branch, you'd see the following:
```
* master
  remotes/origin/master
```

The `*` means the current branch.

To create a new branch named development, use the following command:
```
git checkout -b development
```

The `-b` flag creates the branch. Listing the branches now should show:
```
* development
  master
  remotes/origin/master
```

## Changing branches

It would be best not to commit anything directly to the master branch. Instead, do all your work on the development branch and then merge your work into the master branch whenever you have a new public release.

You are already in your development branch, but if you weren't, the way to switch is as follows:
```
git checkout development
```

> That's the same way you create a branch but without the `-b`.

## Making changes on the development

When making changes, add and commit as usual:
```
git add .
git commit -m "Use a useful comment to identify the commit."
```

The first time you push to your remote, do it as follows:
```
git push -u origin development
```
The `-u` flag stands for `--set-upstream`.  After the first time, you only need to do it like this:
```
git push
```

## Merging development to master

Once your developed code is ready to be merged into your master branch, you can do it like so:

First switch to your local master branch:
```
git checkout master
```

To merge development into master, do the following:
```
git merge development
```

Then push the changes in local master to the remote master:
```
git push
```

## Deleting a branch

If you don't need the development branch anymore, or you want to delete it and start over, you can do the following:

Delete the remote development branch:
```
git push -d origin development
```

Then delete the local branch:
```
git branch -d development
```

The `-d` means delete.