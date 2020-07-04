---
title: Plugins
description: Plugins that are written for TDE
published: true
date: 2020-07-04T16:47:09.291Z
tags: tde, plugins, lua, code, customization
---

# Plugins
Since version `0.6.0` TDE now includes custom plugins. These can be enabled/disabled using `~/.config/tos/plugins.conf` and are stored in `~/.config/tde/`
Each plugin should be inside its own directory.

`TOS` provides some plugins by default if `skel` is installed.
You can visit the example plugins under `/etc/skel/.config/tde`
This file will describe how such plugins work.

First you must understand that their are 2 different kind of plugins.

## Type

1. `Modules`:
 These plugins are simple scripts written in `lua` that are ran in the background under the same process as widgets (thus they can manipulate widgets and more). Users usually don't see these plugins working. Examples are: low battery notifier, keybindings, settings modifiers, file watchers and many more. You can compair then with daemons.
2. `Widgets`: 
 Next you have widgets, These are the plugins you see every day. They are the GUI representation of a certain object. You can see them all over the place. Starting from the clock, package updater, wifi widget, workspaces, settings app and more.
 
When writting a plugin you must take into consideration which one to use.
Everything is allowed in a Module, however when writing a Widget you must inherit `wibox` and return such an object to be consumed by `TDE`

## widgets

# API


