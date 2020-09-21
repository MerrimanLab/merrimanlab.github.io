---
title: Remote Working
author: Murray Cadzow
date: '2020-09-21'
slug: remote-working
categories: []
tags: ["howto"]
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
---

# Merrimanlab Remote Working



## VPN access

Credentials to use are your email/webkiosk

How to Install VPN:
Download Cisco AnyConnect from here: https://www.otago.ac.nz/its/services/network/otago038027.html

**New process**: https://otago.custhelp.com/app/answers/detail/a_id/2113


After you've gone through this process every time you want your home internet to pretend it is at uni open Cisco AnyConnect & tell it to connect to UO-VPN-SSL, then just use your internet like usual.

For students we will have to request your username be allowed VPN access, dependent on whether the uni devises another acceptable way for you to access our servers. 




## Servers

**Need to be on VPN or Uni network to access**

Credentials to use are your Biochem ones, i.e. for printing

General biochem servers
- biochemcompute.otago.ac.nz
- biochemcompute1.otago.ac.nz
- biochemcompute2.otago.ac.nz
- biochemcompute3.otago.ac.nz

Merriman exclusive
- biocmerriserver1.otago.ac.nz
- biocmerriserver2.otago.ac.nz


Important XSan directories:

- Not backed up
    - `/Volumes/scratch/merrimanlab`

- Backed up
    - `/Volumes/archive/merrimanlab`
    - `/Volumes/userdata/staff_groups/merrimanlab`

### Check server load

Check how much a particular server is getting used by looking at the status webpage:

In your web browser use the url `http://<server>:19999`

Avoid servers that are above 75% CPU usage if possible. General biochem servers are likely to have the most people trying to use them.

### ssh and Tmux

Username and password are your biochem credentials
```
ssh <biochem username>@<server>
```

It would be recommended to use Tmux for operating on the server which lets your session remain running even if you disconnect.

Basic usage:

- First login
    - `ssh -t <biochem username>@<server> tmux`
- Subsequent logins
    - `ssh -t <biochem username>@<server> tmux attach`

This will let you close disconnect/close your window without your session on the server closing and let you pick up where you left off.

Additional `tmux` commands can be found here: [tmux shortcuts & cheatsheet](https://gist.github.com/MohamedAlaa/2961058)

### Data transfer

**Avoid using `scp` between your machine and the server while on VPN**


Use instead:
- [Globus](https://globus.org)
- [CloudStor](https://cloudstor.aarnet.edu.au/simplesaml/module.php/discopower/disco.php?entityID=https%3A%2F%2Fcloudstor.aarnet.edu.au%2Fsimplesaml%2Fmodule.php%2Fsaml%2Fsp%2Fmetadata.php%2Fdefault-sp&return=https%3A%2F%2Fcloudstor.aarnet.edu.au%2Fsimplesaml%2Fmodule.php%2Fsaml%2Fsp%2Fdiscoresp.php%3FAuthID%3D_25b3f905755f81a69c5bd3d714ad9c8066d93b9116%253Ahttps%253A%252F%252Fcloudstor.aarnet.edu.au%252Fsimplesaml%252Fmodule.php%252Fcore%252Fas_login.php%253FAuthId%253Ddefault-sp%2526ReturnTo%253Dhttp%25253A%25252F%25252Fcloudstor.aarnet.edu.au%25252Fplus%25252F&returnIDParam=idpentityid) (similar to dropbox)
- Citrix reciever (student/staff desktop)
    - Doesn't have access to XSan
- OneDrive (inc MS Teams)
    - Doesn't have access to XSan

#### Globus settings


1. Create new Globus account: https://www.globusid.org/create
2. Download [Globus Connect Personal](https://www.globus.org/globus-connect-personal).
3. On the `File Manager` side tab, search in the `Collection` search bar, search for `UoO_Biochemistry` and use your biochem credentials to log in.
4. Use Globus Connect Personal GUI to connect to biochem server and transfer files.

[How-to article](http://biocconfluence.otago.ac.nz:8090/display/BKB/How+do+i+use+Globus+to+transfer+large+files) on setting up Globus for file transfer to/from biochem server. (might not be current)

#### Cloudstor

Needs (complex?) further configuration in order to be able to transfer data to/from XSan

Similar to dropbox for sending data to/from other people offers 1 TB of storage

### Rstudio server

Use any of these to use RStudio and operate on data that is stored on the XSan

Put url in your web browser after connecting to VPN

https://biochemcompute.otago.ac.nz:8787
https://biochemcompute.otago.ac.nz:8787

https://biocmerriserver1.otago.ac.nz:8787
https://biocmerriserver2.otago.ac.nz:8787

One of the limitations is that you can only have a single project open at a time and if you use a different server url it disconnects the current server and loads the project up in the new one (possible benefit if the server your start on starts to get used heaps you can migrate servers easily)

## Social connectivity

### Zoom


How to install Zoom:
The uni has made a video here: https://blogs.otago.ac.nz/zoom/how-to-install-zoom/

When (if) zoom lab meetings go ahead I will send around a meeting link for everyone to connect to the zoom call - all you will need to do is click this link & your zoom app should open and ask if you want to link to the meeting.

For those whose turn it is to present I will send more info about how to share your slides with everyone too.

You will need to check that your computer has a microphone to use zoom - if you do not have a web cam on your computer that is no drama, you should still be able to share your computer screen via zoom (if you're presenting), and see everyone else, we just won't be able to see you. 🙁

Log in using the 'SSO' option and use "otago" as the zoom domain. You'll be taken to a Uni log in portal where you use your email/webkiosk credentials

#### Create on demand Zoom meetings

Person creating:
1. Open Zoom
2. Click "Start with Video" or "Start Without Video"
3. Click invite (at bottom)
4. email or copy the url (bottom left of window)

Joining:
1. click the shared url

#### Zoom tips/tricks

https://www.groovehq.com/blog/zoom-tips-and-tricks


### Slack

Credentials are whatever you used when joining up

workspace: merrimanlab.slack.com

### MS Teams

This is what the Uni is trying to transition to for things like having shared word/excel documents

It is extremely similar to slack except:
- live file sharing/collaboration
- conversations on documents
- chat history doesn't have a limit and is saved
