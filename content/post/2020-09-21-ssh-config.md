---
title: ssh config
author: Murray Cadzow
date: '2020-09-21'
slug: ssh-config
categories: ["bash", "tips"]
tags: ["bash","ssh","tips"]
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
---

This post is about setting up an ssh config to make logging into remote machines easier.

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

You can add additional host entries by copying that of biochemcompute and modifying `Host` and `HostName` for the other servers you wish to be able to log into.

3. Close and save with `<ctrl> + x`, `y`, `<Enter>`

4. Ensure the permissions are correct by running `chmod 600 ~/.ssh/config`.

Once you have made this file you can now `ssh` onto the servers by `ssh <Host>`, e.g. `ssh biochemcompute` and the config file takes care of the rest of the details.

you can also use it for `scp`, such as `scp biochemcompute:/path/to/your/file /path/to/put/file` to copy a file from the server storage to your local machine.