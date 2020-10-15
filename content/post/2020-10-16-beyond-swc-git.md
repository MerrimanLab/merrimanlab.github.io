---
title: "Beyond Software Carpentry Git"
date: 2020-10-16T10:14:49+13:00
author: "Riku Takei"
draft: false
weight: 0
enableToc: true
tags: ["git"]
tocLevels: ["h2", "h3", "h4"]
---

## Intro

Once you have finished [Software Carpentry Git lesson](http://swcarpentry.github.io/git-novice/), you should be able to do basic version controlling with Git. You would have learnt how to:
- Stage and commit your changes
- Make a remote GitHub repository
- Push and pull from your remote repository
- Solve merge conflicts

These skills are enough for you to get started with tracking and maintaining your own project, but once you start collaborating with others, you can run into trouble really quick. Even if you don't encounter major problems, knowing a little bit more beyond SWC git lessons will help you streamline the process of how you maintain your repository.

## Collaborate with a fork, not a clone

Towards the end of the SWC git lesson, you would have touched briefly on collaborating with others. In the lesson, you would have had to add your partner to your list of collaborators on GitHub, which allowed you to `clone` it to your computer for editing. However, in reality, the project manager/maintainer will **not** add you as a collaborator and allow you full access to the repo.


Instead, you are encouraged to _fork_ the repository onto your remote first, and then make changes to the forked repository. The difference between this approach compared to cloning is that you are able to change the repo as much as you like without accidentally breaking the original repository that you have forked from.

### Fork vs. Clone

To further clarify, a **fork** is a _copy_ of the original repo (owned by someone else) that has been copied over to your remote account, which **you are now the owner of** (the forked repo). A **clone** is a copy of the original repo (owned by someone else) that has been copied to your local computer which you can edit, but **still owned by someone else**, and therefore you won't be able to push your changes to the repo (unless you are a collaborator).

## Send changes with Pull Requests (PR)

So, then, how do you add your changes to the original repo when you don't have access to it?

GitHub is smart enough to keep track of all the forked repos of the original repository, and when it finds out that there are differences between the original and the forked repo, it will ask if you want to "send a pull request". A **Pull Request (PR)**, as the name suggests, asks the original repo owner to **_pull_** the changes from your forked repo into their repo.

**A general rule of thumb is to limit a PR to changes that achieve a specific goal (e.g. to fix a particular bug).**

When a PR is made, the owner will be able to check the changes you have made to the repo, and decides to add (merge) it to the repo or reject it. PR gives that extra layer of checking and reviewing in order to prevent the program from breaking and reduce the chance of introducing bugs.

### How PR works

PRs work by comparing the changes between a specific "branch" of the original repo with a specific branch in your repo. When more changes are requested during the review process of your PR, you can simply add more commits to the particular branch on your fork, and the changes will automatically be reflected in the PR. This may sound convenient, but it can get very messy very quick if you don't be careful.

### When PRs get messy

Since PRs depend on the changes made to the branch, if you commit changes to that same branch without thinking, those changes will get added to the PR **even if the changes are completely unrelated to the PR you have made**.

In order to fix this, you will have to revert your commit, add the changes you were requested, wait for the changes to get merged, then somehow remember what changes you have made before reverting the commit - all the extra work you could have prevented, had you known about **branches**.

## Branches

A `branch` is a temporary copy of the current state of the repo in a different "workspace". In this newly created branch, you will be able to make changes and edit as you wish without affecting the state of the repo when you made the branch.

Sound familiar? That's because a fork is a special type of branch.

Branches allow you to work on different parts of the project without breaking the other parts, and keeps you in control of what changes are being made to the repo.

### Create, delete, and switch branches

To create a new branch:

```{bash}

	git branch <branch-name>

```

To delete a branch:

```{bash}

	git branch -d <branch-name>

```

In order to switch to a different branch, use the `checkout` command:

```{bash}

	git checkout <branch-name>

```

If you don't know what branches are available, use `git branch` without a branch name and you should get a list of all the branches for your repo:

```{bash}

	git branch

```

### Importance of branching

Now, coming back to the messy PR example.

If, before you had sent a PR, you had created a branch (called `bug-fix`) with all the changes that, for example, fixed a bug in the program. Then, you would send a PR using the `bug-fix` branch, and **not** your main working branch. Since the `bug-fix` branch is self-contained and has nothing to do with other branches, you can now work on other parts of the project (hopefully in a new branch) and commit the changes as much as you like without affecting the `bug-fix` branch.

When you are requested to add further changes to your `bug-fix` branch, then you would simply `checkout` the branch, make the changes, and commit the changes to the branch, and the changes will get reflected on the PR.

## Summary

- Fork the repo for collaboration
- Make new branch for specific changes you want to make
- Send pull requests with specific branch

