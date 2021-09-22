---
title: Conda environments
author: Murray Cadzow
date: '2021-09-22'
slug: []
categories:
  - reference
  - howto
  - quickref
tags:
  - howto
  - python
  - reference
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
---


## Using Conda on the server

```
# Make conda available
# latest version installed at time of post
module load miniconda/Miniconda3_4.8.3
```


## Conda environments

Conda is a package manager that can make running software that requires specific versions of software easier to install and manage.

Essentially, conda lets you define an environment with all the dependencies of the software taken care of and then when you want to use the software, you load the environment. This prevents clashes from the global operating system having different versions installed/updated.

### Activate and environment

In order to 
Activate the environment
```
conda activate {env name}
```

You can activate multiple environments to build up to a certain environment if you wish too. To do this activate each environment sequentially. Usually it is more convenient to create a single environment with the needed packages.

### Listing existing environments

Environments are available from three main locations:

- installed at a system level
- installed at a user level
- installed within your current directory

In order to see which environments are already installed and available to you use the following command:

```
conda env list
```

Once you know which existing environment you want to use, activate it using either the name or path

```
# activate an environment called "base"
conda activate base
```

### Creating your own

#### User level

#### Directory level

### Deactivating an environment

To make an activated environment inaccessible to your session use

```
conda deactivate
```
