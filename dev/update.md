---
title: System Updater
description: automatically mitigate Arch Linux manual Interventions
published: true
date: 2020-03-14T22:00:36.582Z
tags: tutorial, help, update, dev
---

# System Updater
Before we get started. Here is a quick list of everything you should know.

[The repo](https://github.com/ODEX-TOS/system-updater)


Before you want to commit something to this repo you first have to understand what this repo is used for.

To keep it simple. The `system-updater` utility is a wrapper around the `package-manager`

Upstream (Arch Linux) occasionally release package updates that require manual intervention.
[RSS FEED of interventions](https://www.archlinux.org/feeds/news/)

This script aims to automatically resolve these issues.

# What does what

## packages
This file contains a list of all required packages to be installed.
It will detect packages that are not installed but needed. This happens because a user can break the install.

## system-updater.conf
This is a general configuration file.
In case user really don't want to have a package installed they can blacklist it here

## tos-latest.info
This file will be pulled from the server and displays to the user what will happen when the script is ran.
For example fix bugs (by updating to the latest version) or perform interventions from upstream

## system-updater.sh
This is the root executable. It is responsible for the "boilerplate code"
It contains the following

- help page
- command line parser
- way to get custom data from the servers

> should almost never be touched
{.is-info}

## update.sh
This file gets pulled from the network when running `system-updater`
It contains the code that is executed to `fix` the system.


> Be aware that pulling code from the net is dangerous. Because of this extra steps are taken that this git repo is never compromised.
> The reason that this is launched on the net is because of the broken package manager that **requires** manual intervention. Thus we cannot push the code to the repository since that will not be updated by the package manager
{.is-danger}

All custom code should usually be added to `update.sh`