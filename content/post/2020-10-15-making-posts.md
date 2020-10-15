---
title: "A beginner's guide to making new posts with Hugo"
date: 2020-10-15T11:42:39+13:00
author: "Riku Takei"
draft: false
weight: 0
enableToc: true
tags: ["hugo", "markdown"]
tocLevels: ["h2", "h3", "h4"]
---

## What is Hugo?

[Hugo](https://gohugo.io/) is a popular **_static site generator_** written in Go language. It's static in the sense that the web pages do not change depending on who visits the site (for example, contents do not change for different user) -- all the contents displayed on the site are pre-defined. Given a bunch of markdown files, Hugo automatically convert those markdown files into HTML files that are ready for a web site.

The aim of this post is to give you an introduction on how Hugo works and how to make a new post for the website.

Please note that this post is a **_Hugo_** workflow, and there is another workflow based on R `blogdown` package that is geared more towards compiling RMarkdowns.

## Install relevant files and programs

### Install Hugo

Install the latest version of Hugo for your OS. If you have `homebrew` in MacOS, the following will install Hugo:

``` {bash}

	brew install hugo

```

### Download this blog

Download the repository for this blog to a directory of your choice:

``` {bash}

	cd /directory/of/your/choice
	git clone git@github.com:MerrimanLab/merrimanlab.github.io.git

```


## Structure of Hugo blogs

The 2 directories you have to be aware of are:
1. `content/`
1. `public/`

#### Content directory

The `content/` directory is where you put your markdown files in - i.e. your blog contents.

You will notice that the content directory is organised into several sub-directories, such as `about/` and `docs`. These directories are the specific groupings the author of the blog has decided to make in order to organise the posts within the blog.

You can make your own "group" by making a  new sub-directory and make it appear on the blog, but this is beyond the scope of this post.

#### Public directory

The `public/` directory is where Hugo places the converted HTML files in. In other words, this directory contains your web site.

You will notice that there are (or will be) directories within the `public/` directory that matches with the `content/` sub-directories (e.g. `public/about/`). Each of these sub-directories reflect the counterparts in the `content/` directory and organised accordingly.

## How do I make a new post?

As you may have guessed already, in order to make a new blog post, the only thing you need to do is to create a markdown file and place it in the correct directory in `content/`.

To do this, use Hugo's `new` command:

```{bash}

	hugo new content/path/to/content/new_post.md

```

This will create a `new_post.md` file in the directory that you have specified. Ideally, you will want to add a date to your file name in order to prevent duplicate file names within the `content/` directory.

You can now edit the markdown file as you wish.

## How do I look at the post I just made?

First, you will have to generate the website with Hugo:

```{bash}

	hugo

```

The above command will create the `public/` directory (if it isn't present already) and everything required for a website in it. However, it will **not** show you what it looks like. To look at what the website looks like, use the `server` command:

```{bash}

	hugo server

```

This will give you a local adress which you can copy and paste into your web browser to have a look at.

## I'm happy with my post - what next?

When you are finished with your post, commit your markdown file with `git` and push it to the blog repository (not covered in this post).

