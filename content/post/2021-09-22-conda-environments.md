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

Full documentation on managing conda environments can be found at [https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)

There are two main types of environment that I would deem "globally accessible" (installed at a system or user level) or "directory specific" (installed into a specific directory).



### Listing existing environments

Environments are available from three main locations:

- installed at a system level
- installed at a user level
- installed within your current directory

In order to see which environments are already installed and available to you use the following command:

```
conda env list
```

### Activate and environment

Once you know which existing environment you want to use, activate it using either the name or path

A system/user environment would be activated like this:
```
# activate an environment called "base"
conda activate base
```

If a directory has a specific environment to be used, it is convention for it to be in `./envs/` within your working directory, refer to the section on [creating directory specific environments](create-dir-env).
A directory specific environment would be activated like this:
```
# activate a directory specific environment
conda activate ./envs
```


You can activate multiple environments to build up to a certain environment if you wish too. To do this activate each environment sequentially. Usually it is more convenient to create a single environment with the needed packages.





### Creating your own

#### System level

System level environments can only be created/modified by admins.

#### User level

User level environments are very useful if there is a set of common packages you use together often. When these are created they get stored within your home directory which means that other users won't be able to access them.

```
conda create --name <environment_name> <packages>
```

If you want to add packages later you can use
```
conda install --name <environment_name> <packages>
```


You can also create environments with specific package versions:
```
conda create --name <environment_name> <package>=<version>
```

#### Directory level {# create-dir-env}


Creating directory specific environments is essentially the same as creating system or user environments, except instead of the `--name` flag, you use `--prefix` and provide the directory name of where you want to install it into - usually `./envs/`. One of the benefits of using directory specific environments is that anyone that can access the directory will be able to use the environment that is specifically designed for that directory.

```
conda create --prefix ./envs <packages>
```

### Deactivating an environment

To make an activated environment inaccessible to your session use

```
conda deactivate
```
