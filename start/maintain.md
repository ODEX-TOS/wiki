---
title: Maintanance
description: What should you do if you wish to maintain your TOS environment
published: true
date: 2020-03-16T19:18:26.611Z
tags: tos, getting started, tutorial, maintain
---

# Maintanance

If you want to use `TOS` over an extended period of time you have to know how to maintain your system.

Below is a list of 'things' you can use/do to maintain your system.

## Package Manager

As most of you know each system has quite a bit of software.
The backend responsible for managing your software is called `pacman`

> We don't recommend to use pacman directly as tos has mechanisms ontop of it to help you with maintaining your system.
{.is-warning}

If you wish to know more about updates read [this](/user/update)

### Cleanup data

Our package manager keeps data of outdated packages in case you ever wish to do a rollback.
However when your system is low on diskspace you can issue this command

```bash
tos -Sc # sync clear removes old cache data
```

### Remove unused packages

In case you have installed something that you don't want to use you can do the following.

```bash
tos -Rns <package>
```

It will remove the package and all of its dependencies that are no longer used.

## Broken updates

Sometimes an update is broken.
This is due to the backend called `pacman` and is usually because of a specific package.

However TOS has a fix for this.
We have a program called `system-updater`
More information can be found [here](/user/update) under the `system-updater` section

To fix the broken package issue simply run 
```bash
system-updater
```

It will fix the broken package and ontop of that update your system to the latest version of `tos`

> If this command doesn't resolve your issue then you have 2 options
> You can either wait for a fix from the developers of TOS
> Or have a look over [here](https://www.archlinux.org/news/)
> They usually tell you what the problem is and you can manually try to solve it
{.is-info}

## Broken configuration

If some effect is not working as expected and an update hasn't fix your issue it can be one of 2 things.

- A bug is present in the upstream application
- A configuration file is broken

If your issue is the first the you need to supply a bug report to the developers of that application.

However the second issue has 2 fixes.

First make sure that all `tos` config files are correct. More info [here](/user/config).

The second fix you can do is lookup online how the config file you are changing can be fixed.

> If nothing works try to talk with the developers or wait until an update fixes your specific issue
{.is-info}

