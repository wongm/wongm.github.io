---
layout: post
title: Fixing a 'Packages with upgradable origin but kept back' error on Ubuntu
author: wongm
comments: true
tags: [ubuntu, linux]
excerpt_separator: <!--end_excerpt-->
---

After a recent upgrade of my Ubuntu server, every time I tried to run `apt-get upgrade` I ran into a mysterious error - `"Packages with upgradable origin but kept back"`. So how to fix it?

<!--end_excerpt-->

**Some detective work**

I had a closer look at the error message.

```
Packages with upgradable origin but kept back:
 Ubuntu bionic-security:
  ntp
```

So what happens if I try to manually install the `ntp` package?

```
user@server:~# sudo apt install ntp
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 ntp : Depends: libopts25 (>= 1:5.18.12) but 1:5.18.7-3 is to be installed
       Recommends: sntp but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

That error message references the `sntp` and `libopts25` packages - let's try the first one.

```
user@server:~# apt-get install sntp
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 sntp : Depends: libopts25 (>= 1:5.18.12) but 1:5.18.7-3 is to be installed
        Breaks: ntp (< 1:4.2.8p10+dfsg-3+exp3) but 1:4.2.8p4+dfsg-3ubuntu5.10 is to be installed
E: Unable to correct problems, you have held broken packages.
```

A different kind of issue with the `ntp` package and the `libopts25` dependency. So what happens if I try to install `libopts25` directly?

```
user@server:~# apt-get install  libopts25
Reading package lists... Done
Building dependency tree
Reading state information... Done
libopts25 is already the newest version (1:5.18.7-3).
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
```

That's a tad weird - it thinks 1:5.18.7-3 is the newest version, not the higher numbered 1:5.18.12.

Off to the [Ubuntu package repository](https://packages.ubuntu.com/bionic/libopts25) to look at `libopts25`, and what's the latest version?

```
1:5.18.12-4
```

Higher than the 1:5.18.7-3 version that my machine thinks is the newest. 

What happens if I try to install that specific version?

```
user@server:~# sudo apt install libopts25=1:5.18.12-4
Reading package lists... Done
Building dependency tree
Reading state information... Done
E: Version '1:5.18.12-4' for 'libopts25' was not found
```

Weird - it doesn't exist.

Time for more debugging - what packages are avilable via my configured package repositories?

First up, `ntp`.

```
user@server:~# sudo apt-cache madison ntp
       ntp | 1:4.2.8p10+dfsg-5ubuntu7.3 | http://security.ubuntu.com/ubuntu bionic-security/universe amd64 Packages
       ntp | 1:4.2.8p10+dfsg-5ubuntu7.3 | http://security.ubuntu.com/ubuntu bionic-security/universe Sources
```

Looking fine - it exists on the `bionic-security/universe` repository.

And `libopts25`?

```
user@server:~# sudo apt-cache madison libopts25
```

Nothing!

**Time to fix my package sources**

The `/etc/apt/sources.list` configuration file details the URLs for remote repositories from where software packages and applications are installed.

So which repository is the `libopts25` package published to? This URL has a clue:

```
https://packages.ubuntu.com/bionic/libopts25
```

Maybe the `bionic` repository?

I fired up my text editor and opened `/etc/apt/sources.list`, and found two package sources that had `bionic` name, but were commented out. Time to uncomment them.

```
## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team. Also, please note that software in universe WILL NOT receive any
## review or updates from the Ubuntu security team.
deb http://mirrors.digitalocean.com/ubuntu/ bionic universe # disabled on upgrade to bionic
deb-src http://mirrors.digitalocean.com/ubuntu/ bionic universe # disabled on upgrade to bionic
```

I then ran `apt-get update` to update the package sources, then tried again to install the `libopts25` package.

```
user@server:~# sudo apt install libopts25
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages will be upgraded:
  libopts25
1 upgraded, 0 newly installed, 0 to remove and 41 not upgraded.
Need to get 58.2 kB of archives.
After this operation, 0 B of additional disk space will be used.
Get:1 http://mirrors.digitalocean.com/ubuntu bionic/universe amd64 libopts25 amd64 1:5.18.12-4 [58.2 kB]
Fetched 58.2 kB in 0s (646 kB/s)
(Reading database ... 141687 files and directories currently installed.)
Preparing to unpack .../libopts25_1%3a5.18.12-4_amd64.deb ...
Unpacking libopts25:amd64 (1:5.18.12-4) over (1:5.18.7-3) ...
Processing triggers for libc-bin (2.27-3ubuntu1.2) ...
Setting up libopts25:amd64 (1:5.18.12-4) ...
Processing triggers for libc-bin (2.27-3ubuntu1.2) ...
```

And success - the `libopts25` package was now installed, and the `apt-get upgrade` command completed without any errors.
