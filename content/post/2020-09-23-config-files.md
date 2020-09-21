---
title: config files
author: Murray Cadzow
date: '2020-09-23'
slug: config-files
categories: []
tags: []
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
draft: true
---

Config files, sometimes referred to as dot-files, are files that you can make to customise the way a program behaves. Two such files you might like to create are `.bashrc` to customise how your bash looks and behaves, and `.Rprofile` to customise how R looks and behaves. Usually these files live in you home directory (`~/`) and because they have a '.' at the start are hidden from view by default but in bash you can view these hidden files with `ls -A ~/`.

# Bash

`.bashrc` is the common file that controls your bash set up. Some systems (such as MacOS) also have a file `.bash_profile`. If your system uses the `.bash_profile` file, you can make it refer to `.bashrc` by having this as the contents of `.bash_profile`:

```
[[ -r ~/.bashrc ]] && . ~/.bashrc
```


## Custom prompt

Creating your own prompt in bash can be really useful rather than having a straight `$`. http://ezprompt.net provides a nice way of modifying your prompt and providing the code to add to your `.bashrc`.

Things you might want to do:
- add your username
- add the hostname (the name of the computer)
- add the current directory
- add the full path to the current directory
- have colour

## Exported variables



## Aliases

Aliases can be quite useful for common commands and arguments you run.

For instance I have aliases set up to ssh onto the server and connect to a tmux session if one is already running.
```
alias merritmux1='ssh -t biocmerriserver1 tmux attach'
```

# R

## Libraries

In general you want to avoid calling libraries that are involved in analyses because this can alter how reproducible your code would be if you passed it to someone else that didn't have your .Rprofile - e.g. don't have `library(tidyverse)` in you .Rprofile. It can be useful to automatically load helper packages such as `devtools` and `usethis` since they aren't used for analysis but are extremely helpful to have loaded when you want to set things up.

