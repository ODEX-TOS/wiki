---
title: Updates
description: How should you update your system
published: true
date: 2020-03-14T20:52:38.575Z
tags: user, tutorial, maintain, update
---

# Updates
In order for you to use your system you need to know how to install software. The basics will be covered here. In case you need to know more you can always look up pacman [wiki](https://wiki.archlinux.org/index.php/Pacman)

# Tos package manager
Open up a terminal (mod+Enter) and type in the following
```bash
tos -Ss <query> # where query is the application to lookup
```
You will get a reply containing all matches to your query. Find the correct application name and the type the following

```bash
tos -Syu <name> # where name is the name of your application.
```

Lets take a look at a simple example.
We will be trying to install gimp. For those who don't know what gimp is. Its a editor for pictures much like photoshop.

```bash
tos -Ss gimp # gimp will most likely be named gimp
>extra/xsane-gimp 0.999-3 (248.1 KiB 772.0 KiB)
>    XSane Gimp plugin
>extra/xsane 0.999-3 (1.5 MiB 4.8 MiB)
>    A GTK-based X11 frontend for SANE and plugin for Gimp.
>extra/potrace 1.16-1 (85.0 KiB 246.0 KiB) (Installed)
>    Utility for tracing a bitmap (input: PBM,PGM,PPM,BMP; output: >EPS,PS,PDF,SVG,DXF,PGM,Gimppath,XFig)
>extra/gimp 2.10.14-1 (18.9 MiB 104.3 MiB) (Installed)
>    GNU Image Manipulation Program

tos -Syu gimp # install gimp
```

As we can see above the package is named gimp `extra/gimp 2.10.14-1 (18.9 MiB 104.3 MiB) (Installed)` We can see that it is inside the extra repository and that the download size is `18.9MiB` with an install size of `104.3MiB`

> You can do much more. For example see what is already installed, update your system, remove packages and more. In case you need to know more you can always look up pacman [wiki](https://wiki.archlinux.org/index.php/Pacman)
{.is-info}

## Keep your system up to data
All you need to do is run `tos -Syu` often enough to keep your system up to date.
If this is not the case tye `system-updater` to perform a version upgrade of tos

> It is important that you update frequently. Since tos is a rolling release distribution you will need to update at least once a week otherwise your updates will become very large.
{.is-danger}


