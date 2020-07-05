---
title: Terminal
description: Explain the customization of st the TOS terminal
published: true
date: 2020-07-05T13:20:46.336Z
tags: terminal config
editor: markdown
---

# Terminal
This document won't cover how terminal emulators or shells work. However it will cover the "things" TOS implemented into their own terminal.

![terminal-emulator.png](/images/terminal/terminal-emulator.png)

> The picture above is an example of how the default `TOS` terminal emulato looks
{.is-info}

## Emulator

The default Terminal Emulator is st (Simple Terminal). This emulator is forked from [suckless](https://suckless.org)

It has a few quirks when comming from other terminal emulators.
First of all the keybindings:

- `alt-l` open url

- `alt-y` copy url

- `alt-o` save last command

- `alt+c` copy

- `alt+v` past

This emulator is design to be simple. It has no dependencies, looks good and is blazing fast.

## Shell

The default shell is [zsh](https://en.wikipedia.org/wiki/Z_shell) with the [oh-my-zsh](https://ohmyz.sh/) framework build in.

Features of this shell are:

- Autocomplete
- History
- Advanced tab completion (better than bash)
- POSIX compliant shell (same as bash)
- Extensibility
- Theme compatibility

And many more

To edit the shell you normally edit `.zshrc` to change the behaviour
However TOS opted for another way.
With a TOS install there should be a `~/.oh-my-zsh/load` directory.
This directory contains shell scripts that should be executed on start of a shell.
All scripts directly in `~/.oh-my-zsh/load` will be executed after `oh-my-zsh` has loaded in.
All scripts in `~/.oh-my-zsh/load/preload` will be executed before `oh-my-zsh` has loaded in.

Usually you use `preload` to configure `oh-my-zsh` and use `load` for everything else.

### useful features

This section covers features that tos implemented extra into zsh.
- Changing the theme is done in `~/.oh-my-zsh/load/preload/theme.sh`
- Use the `password` command to generate a password of length 12
- Use `cpu`, `ram` and `disk` to get basic information
- Use `extract /path/to/tarball` to extract any possible tarbal eg .tar.xz, .zip, .rar etc
- Use `background sleep 5` to put the command `sleep 5` into the background

## Change shell and emulator

You can change the terminal emulator or shell if you are not satisfied with them.

To change the shell call `chsh` and enter the new shell you want to use eg `/bin/bash`
> When giving in a new shell you must give an absolute path otherwise the shell will be broken.
> You can see a list of shells in `/etc/shell`
{.is-danger}

After having changed your shell you must log out and log back in.

In case you wish to use another terminal emulator you must first install one.
Example: 

```bash
tos -Syu alacritty
```

Afterwards you must edit `.xprofile` to add the following line:
```bash
export TERMINAL="alacritty"
```

Where `alacritty` is the name of the terminal emulator
After loggin out and back in all terminal emulators should be the new emulator you are using.

> Entering an invalid emulator as `TERMINAL` will result in no longer opening terminals.
> If this situation happens to you then use `ctrl+alt+f2` to enter a tty.
> Log in with your username and password then change `.xprofile` and reboot the machine
{.is-danger}

