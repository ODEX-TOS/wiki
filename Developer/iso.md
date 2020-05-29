---
title: Live Iso
description: Everything needed to build the live iso
published: true
date: 2020-03-19T23:28:41.185Z
tags: tutorial, help, dev
---

# Live ISO

To build a `tos iso` you will need an arch based distro (as some tools depend on that)

But you will also need the below software

- curl
- GNU sed (supports sed-i option)
- makepkg/repo-add (Arch build scripts)

> To build the server you should be running an Arch Linux based system.
> We prefer you run everything on TOS for the best compatibility.
{.is-info}

All the tools provided in the repository will download and install all necessary packages except for the following.

```bash
pacman -Syu archiso # only dependency not installed by our tools
```

Before building the live iso make sure you are on the latest kernel

```bash
uname -r
# should equal
tos -Sl tos | grep linux-tos | head -n1 | awk '{print $3}'
```

## usage

### Tos iso

Before building the iso you could add your personal repository to the `pacman.conf` file located at `tos-live/toslive/pacman.conf`
You will see the following entry:

```bash
[tos]
Server = https://repo.odex.be
```

This will use the official repository for tos. If you want to use your own repo change the above to

```
[tos]
 SigLevel = Optional TrustAll
 Server = file:///home/zetta/tos/repo/arch/
```

When using a server to serve your repository you can use the following

For more information about the repo read [here](/dev/repo)

This repository also includes the build tool to make iso's. The tool can be found under `toslive/start.sh`

It can be activated as followed

```bash
sudo ./start.sh -s # build the server iso
sudo ./start.sh -awesome # build the desktop iso

sudo ./start.sh -s -a # build the server iso with the keyboard layout to azerty
sudo ./start.sh -awesome -a # build the desktop iso with the keyboard layout to qwerty

# for more information use the help page
sudo ./start.sh -h
```

> start.sh needs to be activated in the same folder or your images won't get build. You also need to be on the latest kernel for the build to succeed


## Structure

Our project has the following files you need to know about:

#### toslive/packages.x86_64_awesome

This file contains all packages required for the operating system (for the desktop edition)

> The packages listed here do not work with the aur see repo below for more info

#### toslive/packages.x86_64_server

This is the same as above but for the server edition (without a desktop)

#### toslive/airootfs/root/customize_airootfs.sh

This file is a script that will be executed during build. In this file you "prepare" the operating system for the user

> Do not install packages from the aur here. It won't work

#### toslive/packages.x86_64_awesome

This file contains the packages that are going to be installed in the live environment.
You should add an extra line if you want to install an extra package.

> Do not install packages from the aur here. It won't work.

#### toslive/version-edit.txt

This contains the current version of our operating system. When making a pull request please update it accordingly.

#### toslive/out

This directory will contain all our iso's

#### toslive/images

This directory is an archive for our old images

#### toslive/start.sh

The script that actually builds the iso's

