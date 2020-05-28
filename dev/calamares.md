---
title: Installer
description: Extend the Calamares Installer
published: true
date: 2020-03-16T20:01:27.945Z
tags: help, dev, installer
---

# Installer

The git repository for this section can be found [here](https://github.com/ODEX-TOS/installer)

To get more information about the used framework look [here](https://calamares.io/about/)

To give you a short description:
Calamares is a distribution independent installer framework that makes it easy and simple to extend and customize for your distribution.

What does that mean for us?
We use Calamares to provide an easy installation.

However other ways of installing `tos` exist.

We also provide a `installer-backend` for people who want to install tos across many systems.
The git repo for this can be found [here](https://github.com/ODEX-TOS/tos-installer-backend)

This article will mainly cover `calamares`
Everything you need to know about the `installer-backend` can be found in that repository or on its own [wiki](https://github.com/ODEX-TOS/tos-installer-backend/wiki)

## Calamares

As described above we mainly use the `calamares` installer.

To build it we recommend you to look at the `PKGBUILD` file added in the repository.

As the install procedure is quite complex.
We also recommend that you run `makepkg` to build the package and run `pacman -U *.pkg.tar.*` followed with `calamares`
to test your builds.

Here is a quick script on how your flow should work

```bash
makepkg # compile calamares
pacman -U installer*.pkg.tar.*
calamares
pacman -Rns installer
```

> Don't forget to edit the `source` array in the `PKGBUILD` file to point towards your repository instead of the git repo in TOS
> The line we are talking about is `"git+https://github.com/ODEX-TOS/installer.git#branch=master"`
> You should change it to this
> `"git+/path/to/local/repo/where/you/are/working/in"`
{.is-info}

## Files

### pacstrap-tos

The default pacstrap command (provided by arch) errors out when `/proc` is already mounted in the `chrooted` environment.
However `calamares` does this when mounting your hard drive.

This custom `pacstrap` command takes the actions of `calamares` into consideration.

### settings.conf

This file describes the default behaviour of Calamares.
It describes to flow of the program in a `yaml` configuration.
IF you are adding new modules you shouldn't forget to add then to this file or otherwise they will not be executed.

### src/branding/tos

This entire directory describes how the UI should look.

[Here](https://github.com/calamares/calamares/tree/master/src/branding) is an example configuration of said branding directory with more information

### src/modules/*

Calamares works with a lot of modules that each do something specific.

For example install and configure the bootloader.
Install packages and more.

[Here](https://github.com/calamares/calamares/tree/master/src/modules) is a list of all possible modules.

## Adding/removing packages from installation

The [file](https://github.com/ODEX-TOS/installer/blob/master/src/modules/packages/packages.conf) you should need to edit

This file contains all information on which package to install or remove.

The `install` array contains packages that must be installed
The `try_remove` array contains packages that are present in the live iso but should be removed from the final product (eg installer software)

## Cleanup final system

We also have a small `shell script` that cleans the final install of `tos`
You can find the script [here](https://github.com/ODEX-TOS/tools/blob/master/cleanup.sh)





