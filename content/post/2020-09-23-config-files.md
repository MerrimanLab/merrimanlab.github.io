---
title: Config files
author: Murray Cadzow
date: '2020-09-23'
slug: config-files
categories: ['config']
tags: ['bash','R','config','tips']
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
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

Exporting variables is a useful way for defining environmental settings. Often this is setting a bash variable to tell programs where to look for things. This website has a few examples of bash variables (https://www.thegeekstuff.com/2010/08/bash-shell-builtin-commands/).


### Module example

For instance on the server to be able to make use of the `module` system, you'll need to add to you `.bashrc` file:
```
export MODULEPATH=/Volumes/scratch/software/modules:$MODULEPATH
```
This lets `module` know where to look for the software modules that are installed/configured.

### Rmarkdown Pandoc

A useful one on the server, is defining where R is going to look for pandoc for compiling RMarkdown documents.

I have the following in my `.bashrc` file
```
export RSTUDIO_PANDOC=/usr/lib/rstudio-server/bin/pandoc
```
When I then open R on the server, the value of that variable is then passed and set to the equivalent in R, and R then knows that I want to use `pandoc` found at that path. This is important because there might be another instance of `pandoc` that is available on your _PATH_. _RSTUDIO_PANDOC_ is the name that R has specified to use if you want to customise which `pandoc` is used.

### Better bash history

Bash records your history as it goes but if you are operating across multiple windows it doesn't work the way you would hope for - e.g. it is only recorded from a single given session, even if you work in multiple. _PROMPT_COMMAND_ is a bash variable that is run as part of running commands. This particular one is designed to time and date stamp commands (not run as root) and their working directory into a daily log file. The logs live in `~/.logs/` so this needs to be made for the command to run `mkdir -p ~/.logs`.

```
export PROMPT_COMMAND='if [ "$(id -u)" -ne 0 ]; then echo "$(date "+%Y-%m-%d.%H:%M:%S") $(pwd) $(history 1)" >> ~/.logs/bash-history-$(date "+%Y-%m-%d").log; fi'
```
If I want to search my logs I can use `grep <command> ~/.logs/*` and it will tell me all the times and directories I ran a command, and how I ran it. The history in these log files is made up of all commands you run on the computer, regardless of how many terminal windows you have open.

## Aliases

Aliases can be quite useful for common commands and arguments you run.

For instance I have aliases set up to ssh onto the server and connect to a tmux session if one is already running.
```
alias merritmux1='ssh -t biocmerriserver1 tmux attach'
```

This particular one does have to have your ssh config set up so that the details for biocmerriserver


It's based on the setup that is required for logging into NeSI (https://support.nesi.org.nz/hc/en-gb/articles/360000625535-Standard-Terminal-Setup)

1. In a new local terminal run; `mkdir -p ~/.ssh/sockets` this will create a hidden file in your home directory to store socket configurations.
2. Open your ssh config file with  `nano ~/.ssh/config` and add the following (replacing <username> with your username):

```
Host *
    ControlMaster auto
    ControlPath ~/.ssh/sockets/ssh_mux_%h_%p_%r
    ControlPersist 1

Host biochemcompute
   User <username>
   HostName biochemcompute.uod.otago.ac.nz
   ForwardX11 yes
   ForwardX11Trusted yes
   ServerAliveInterval 300
   ServerAliveCountMax 2
```

## `ssh` Config

You can add additional host entries by copying that of biochemcompute and modifying `Host` and `HostName` for the other servers you wish to be able to log into.

3. Close and save with `<ctrl> + x`, `y`, `<Enter>`

4. Ensure the permissions are correct by running `chmod 600 ~/.ssh/config`.

Once you have made this file you can now `ssh` onto the servers by `ssh <Host>`, e.g. `ssh biochemcompute` and the config file takes care of the rest of the details.

you can also use it for `scp`, such as `scp biochemcompute:/path/to/your/file /path/to/put/file` to copy a file from the server storage to your local machine.

---

# R

The main config file for R is the `~/.Rprofile`. You can also have project specific `.Rprofiles` that live in your R project directories.


## Server .Rprofile example

This Rprofile is designed to use the shared libraries on the server.

**Edit the _firstname_ _lastname_ spots**

```
# set the default cran repository
options(repos = c(CRAN = "https://cran.stat.auckland.ac.nz/"))

# sets the libpath to be the shared directory if using R > v4.0, or personal if < v4.0
.libPaths(
    c(paste0("/Volumes/scratch/merrimanlab/R/x86_64-pc-linux-gnu-library/",version$major,".", strsplit(version$minor, "\\.")[[1]][1], "/"), # shared merriman library
    paste0("~/R/x86_64-",ifelse(version$major == "4", "pc", "redhat"),"-linux-gnu-library/",version$major,".", strsplit(version$minor, "\\.")[[1]][1],"/") # personal library
)) 

# load 'helper' packages automatically if running 
# an interactive session - i.e. not a script
if (interactive()) {
  suppressMessages(require(devtools))
  suppressMessages(require(usethis))
  suppressMessages(require(testthat))
}


# warn on partial matches
options(
  warnPartialMatchArgs = TRUE,
  warnPartialMatchDollar = TRUE,
  warnPartialMatchAttr = TRUE
)

# fancy quotes are annoying and lead to
# 'copy + paste' bugs / frustrations
options(useFancyQuotes = FALSE)

# set some author info that packages use
options("devtools.desc" = list(
  Author = "firstname lastname",
  Maintainer = paste0("firstname lastname", " <", "email address", ">"),
  License = "MIT + file LICENSE",
  Version = "0.0.1"
))
options("devtools.name" = "firstname lastname")

# use more cores if possible installing packages
options(Ncpus = 8)

```

## Libraries

In general you want to avoid calling libraries that are involved in analyses because this can alter how reproducible your code would be if you passed it to someone else that didn't have your `.Rprofile` - e.g. don't have `library(tidyverse)` in you `.Rprofile`. It can be useful to automatically load helper packages such as `devtools` and `usethis` since they aren't used for analysis but are extremely helpful to have loaded when you want to set things up.