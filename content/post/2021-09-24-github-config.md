---
title: github config
author: Murray Cadzow
date: '2021-09-24'
slug: []
categories:
  - config
  - quickref
  - howto
tags:
  - git
  - reference
  - howto
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
---

From August, github removed the use of authentication for https when using git.

If you had cloned repositories using https rather than ssh you will have issues with pulling and pushing to github.

For documentation about how to change to using ssh to authenticate with github please see [https://docs.github.com/en/authentication/connecting-to-github-with-ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

Once set up, you will then need to either re-clone the repositories (possibly the easiest method), or go through each repository on your computer and update the remotes so they use `ssh` rather than `https`.

You can check what you repo remotes are by using:
```
git remote -v
```

If the url for the remote starts with `https` then it will need to be updated.

Then to remove remotes:
```
git remote remove <remote_name>
```

for example to remove the origin remote you would use `git remote remove origin`

