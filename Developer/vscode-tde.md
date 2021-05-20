---
title: VSCODE - TDE setup guide
description: Setup support for debugging and running tde
published: true
date: 2021-05-20T14:05:45.327Z
tags: tutorial, tde, vscode, ide
editor: markdown
dateCreated: 2020-09-30T13:01:43.646Z
---

# VSCODE setup guide
This guide will help you setting up and installing vscode, lua dependencies and getting tde autocompletion working.

## Download dependencies

Lets begin by downloading vscode

```bash
tos -Syu visual-studio-code-insiders
```

Afterward open the quick launcher (Ctrl+P) and type in the following `ext install sumneko.lua`
This will install the lua linter and autocompletion script

## Configure lua extention

Now you will need to modify the Lua extenstion.
Open `settings.json` by typing (Ctrl+P) followed by settings.json
Once presented with the file remove all json elements that start with `"Lua."`
Afterwards add the following settings to that file
```json
    "Lua.runtime.version": "Lua 5.3",
    "Lua.workspace.ignoreSubmodules": false,
    "Lua.workspace.library": {
        "/usr/share/tde/lib": true,
        "/etc/xdg/tde": true
    },
    "Lua.diagnostics.disable": [
        "lowercase-global"
    ],
    "Lua.diagnostics.globals": [
        "client",
        "awesome",
        "screen",
        "root",
        "awful",
        "mouse"
    ]
```