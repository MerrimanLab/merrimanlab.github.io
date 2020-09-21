---
title: "Search for file under a directory"
date: 2020-09-22T08:50:27+12:00
draft: false
weight: 0
categories: ["blog"]
tags: ["bash",]
enableToc: true
tocLevels: ["h2", "h3", "h4"]
---

# Search for file using "find" command

Per `man find`:

> The `find` utility recursively descends the directory tree for each `path`
> listed, evaluating an `expression` (composed of the "primaries" and
> "operands" listed below) in terms of each file in the tree.

## Search for a file under a directory

You can search for a file with specific name like so:

```{bash}
find /path/to/search -name <name of file/dir>
```

The default `path` to search is the current directory `.`, and it will search recursively until the very end.

## Limit your search to N-depth

You may know how "deep" your file/directory is buried in your home directory, and it may be unnecessary to search recursively down to the bottom.

You should add the `-maxdepth <N>` option to your `find` command to limit the search depth:

```{bash}
find /path/to/search -maxdepth 3 -name <name of file/dir>
```

The above command will search at most three directories below `/path/to/search`.

## Limit your serach to specific type of files

If you want to limit your search to a specific type of file (e.g. only care about directories), then modify the command as below:

```{bash}
find /path/to/search -type d -name <name of file/dir>
```

The option `-type <type>` will limit the search on directories.

Here is a list of other types you may find useful:

Type  | What it will search
----- | --------------------
`f`   | Regular file
`d`   | Directory
`l`   | Symbolic link

