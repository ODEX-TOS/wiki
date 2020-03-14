---
title: Window Manager
description: Extend the functionality of the TOS window manager called Awesome
published: true
date: 2020-03-14T21:43:32.269Z
tags: tutorial, help, dev, wm, window manager
---

# Window Manager

Before we get started. Here is a quick list of everything you should know.

[The repo](https://github.com/ODEX-TOS/dotfiles/tree/master/awesome)
[Awesome Documentation](https://awesomewm.org/apidoc/index.html)

The window manager used is `AwesomeWM` it has a scripting language called `lua`
For more information about lua visit [here](https://www.lua.org/start.html)

## Dependencies

### Required Dependencies

| Name | Description | Why/Where is it needed? |
| --- | --- | --- |
| [`awesome-git`](https://github.com/awesomeWM/awesome) |  Highly configurable framework window manager | isn't it obvious? |
| [`rofi-git`](https://github.com/davatorium/rofihttps://github.com/davatorium/rofi) | Window switcher, application launcher and dmenu replacement | Application launcher |
| [`tryone144's picom`](https://github.com/tryone144/compton/tree/feature/dual_kawase) | A compositor for X11 | a compositor with kawase-blur |

### Optional Dependencies

Dependencies needed to achieve the setup's full potential. These are **optional**.

| Name | Description | Will be used by |
| --- | --- | --- |
| `xbacklight` | RandR-based backlight control application | Brightness widget and OSD |
| `alsa-utils` | An alternative implementation of Linux sound support | Volume widget and OSD |
| `acpi`,`acpid`,`acpi_call` | Show battery status and other ACPI info | Power/Battery Widgets. No need for this if you're not using a laptop |
| `mpd` | Server-side application for playing music | Music widget |
| `mpc` | Minimalist command line interface to MPD | Music widget |
| `maim` | Make image | Screenshot tool |
| `feh` | Image viewer and wallpaper setter | Screenshot previews, wallpapers |
| `xclip` | Command line interface to the X11 clipboard | Will be used in saving the screenshots to clipboard |
| `xprop` | Property displayer for X | Custom titlebars for each client |
| `imagemagick` | An image viewing/manipulation program | Music widget/Extracts hardcoded album cover from songs |
| `blueman` | Manages bluetooth | default launch application for bluetooth widget |
| `redshift` | Sets color temperature of display according to time of day | Blue light widget |
| `xfce4-power-manager` | Manages power | default launch application for battery widget |
| `upower` | upower - UPower command line tool | Battery widget |
| `xdg_menu` or `awesome-freedesktop` | Generates a list of installed applications | Menu Module/Useful for generating app list |
| `noto-fonts-emoji` | Google Noto emoji fonts | Emoji support for notification center |
| `jq` | Command-line JSON processor | Read weather |

### Recommended Packages

+ **Terminal emulators**: `st`,
+ **File Manager**: `nemo`
+ **Editors**: `vim`, `neovim`, or `sublime text` with some plugins
+ **Launchers**: `rofi`

## Keybindings

**Mod4** is the Windows key.

### Launchers

<kbd>Mod4 + Return</kbd> - Launch default terminal  
<kbd>Mod4 + \`</kbd> Launch dropdown terminal  
<kbd>Mod4 + Shift + e</kbd> - Launch default file manager  
<kbd>Mod4 + Shift + f</kbd> - Launch default web browser  
<kbd>Mod4 + e</kbd> - Launch application menu  
<kbd>Mod4 + Shift + r</kbd> - Launch web search rofi  
<kbd>Mod4 + Shift + Escape</kbd> Launch system monitor  

### Navigation

<kbd>Mod4 + F2</kbd> - Open today panel  
<kbd>Mod4 + F3</kbd> - Open notification panel  
<kbd>Mod4 + r</kbd> - Open left dashboard  
<kbd>Mod4 + m</kbd> - Toggle music widget  

### Utilities

<kbd>Control + Escape</kbd> - Toggle system tray  
<kbd>Mod4 + l</kbd> - Lock the screen  
<kbd>Mod4 + t</kbd> - Toggle redshift filter  
<kbd>Mod4 + [</kbd> - Decrease the blur effect  
<kbd>Mod4 + ]</kbd> - Increase the blur effect  
<kbd>Print</kbd> - Take a screenshot  
<kbd>Mod4 + Shift + s</kbd> - Take a selected screenshot  

### Awesome

<kbd>Mod4 + F1</kbd> - Show keybindings  
<kbd>Control + Mod4 + R</kbd> - Reload awesome wm configuration  
<kbd>Control + Mod4 + q</kbd> - Quit awesome wm  


## Basic File Structure

+ `Configuration` contains the keybindings, client rules, tags, rofi and picom configuration, startup applications, credentials storage, and tag-list.
+ `Layout` hold the disposition of the widgets, panels and sidebars.
+ `Module` contains all the standalone extra modular features available. You can disable/enable them without causing errors.
+ `Theme` holds all the aesthetic aspects like colors, fonts, and beautiful configuration. It also contains the wallpapers and icons.
+ `Widget` contains all the widgets and its configurations.
+ `Binaries` contains all the binaries needed for a certain task.


## Configuration and Preferences

+ **Configure theme's colors/aesthetics?**

	Awesome WM uses the `beautiful` library to beautify your setup.

	Change the values here:
	- `awesome/theme/default-theme.lua`
	- `awesome/theme/theme-name/init.lua`

+ **Configure Panels and bars?**
	
	The panels and sidebars are located in:
	- `awesome/layout/`
	- `awesome/widget/right-dashboard/`

	Top panel location:
	- `awesome/layout/top-panel.lua`

	Left panel location:
	- `awesome/layout/left-panel/init.lua`

	Right/Notificaton panel location:
	- `awesome/widget/right-dashboard/right-panel.lua`

	The right/notification panel is optional, you can remove it from the top panel.

+ **Configure Start-up and default applications**
	
	You can change the applications here:
	- `awesome/configuration/apps.lua`

+ **Configure Keybindings?**
	
	You can check keybinds by pressing `Mod4 + F1`.

	Client keybindings:
	- `awesome/configuration/client/keys.lua`

	Global keybindings:
	- `awesome/configuration/keys/global.lua`

+ **Configure client rules?**
	
	The client rules manages the behaviour of the client. Is it floating? Is it above the other clients? Is it under the other clients, perhaps? What tag will the client spawn?

	Client rules:
	- `awesome/configuration/client/rules.lua`

+ **Configure client tags?**
	
	Tags are the "workspaces". Terminal, web browsers, text editors are few of the tags used here.

	Client tags:
	- `awesome/configuration/tags/init.lua`

+ **Configure the compositor?**
	
	The compositor we are using is tryone144's picom [feature/dual_kawase](https://github.com/tryone144/compton/tree/feature/dual_kawase) branch that provides the `kawase blur` shader. It gives the beautiful<sup>**beauty is subjective**</sup> blur effect.

	Compositor configuration file:
	-  `awesome/configuration/picom.conf`

+ **Configure rofi?**
	
	What is rofi? Rofi is a window switcher,  application launcher, ssh dialog and dmenu replacement

	Rofi location:
	- `awesome/configuration/rofi/`

	We will use two rofi configuration. One is for application launcher and the second one is for web searching.

	Rofi Application Launcher:
	- `awesome/configuration/rofi/appmenu/rofi.rasi`

	Rofi Web Search:
	- `awesome/configuration/rofi/sidebar/rofi.rasi`

## About Widgets

#### Calculator Widget
	
The calculator widget is the result of my boredom. 
- Supports:
	- Basic math operations
	- Keyboard support

- Tips:
	Enable keyboard support by hovering your mouse above the calculator.
	Or toggle it on/off by pressing the keyboard button.
	Only numbers, arithmetic operators, and decimal point is accepted.

- Keyboard Binding:
	- `=` and `Return` to evaluate.
	- `BackSpace` to delete the last digit.
	- `Escape` to clear the screen.
	- `x` stops the keygrabbing.

- Note:
	- While in keygrabbing mode, your keyboard's focus will be on the calculator. So you're AwesomeWM keybinding will stop working.<sup>temporarily of course.</sup> 

- Stopping the keygrabbing mode:
	- Move away your cursor from the calculator.
	- Toggle it off using the keyboard button.
	- Press `x`.


#### Trash Widget

The trash widget.. well errm.. is actually useful.
It monitors your trash directory using the AwesomeWM's `awful.spawn.with_line_callback()` and `gio monitor trash:///`, then updates the icon if there is changes.

- Tip:
	Right-click to show the menu.


#### Music Widget

- Depends:
	- `playerctl`, `curl`

- Optional Depends:
	- music file with metadata
	- music file with hardcoded album cover

It is better if you have a music file with metadata.


#### Screen Recorder Widget

This is actually useful for basic screen recording.

- Depends: `ffmpeg`

- Features:
	- You can toggle microphone on and off.
	- Change the settings on the main widget.

- Note: 
	- You can change the default settings in `awesome/widget/screen-recorder/screen-recorder-config.lua` 



### About Modules


#### Backdrop Module

This module is developed by [PapyElGringo](https://github.com/PapyElGringo/) for his [material-awesome](https://github.com/PapyElGringo/material-awesome). This module adds a backdrop blur to the dialogs and modals. You can disable it by setting the `draw_backdrop` to `false` in the `awesome/configuration/client/rules.lua`