---
title: Configuration
description: Configur/tweak your system
published: true
date: 2020-04-27T07:08:15.393Z
tags: user, tutorial, maintain, config
---

# Configuration

Lets talk about configurating your environment.

Tos has several tools and packages that you can configure independently.
Each section is described below.

## Window Manager

The official window manager used by tos is `awesomeWM` specifically the package `awesome-tos`
Make sure this package is installed before continuing with this section.

### Colors

If you wish to edit the colors of the window manager then you need to edit the `.config/tos/colors.conf` file.
In case it doesn't exist yet use the following file

```bash
# possible colors for primary accent and background are
# red pink purple hue_purple indigo blue hue_blue cyan teal green  hue_green
# lime yellow amber orange deep_orange brown grey and blue_grey, light can also be used to show white colors
primary="purple"
accent="light"
background="blue_grey"

#colors work as followed
#they follow the HTML spec as the following format
# RRGGBB or RRGGBBAA in hex

#You can override the primary, accent and background color pallets if you want
#The pallete shown below is the purple pallet. For more information visit material design
#primary_hue_50='F3E5F5'
#primary_hue_100='E1BEE7'
#primary_hue_200='CE93D8'
#primary_hue_300='BA68C8'
#primary_hue_400='AB47BC'
#primary_hue_500='9C27B0'
#primary_hue_600='8E24AA'
#primary_hue_700='7B1FA2'
#primary_hue_800='6A1B9A'
#primary_hue_900='4A148C'
#primary_hue_A100='EA80FC'
#primary_hue_A200='E040FB'
#primary_hue_A400='D500F9'
#primary_hue_A700='AA00FF'

#accent_hue_500="F06292"
# accent_hue_50='000000' to set the accent pallet
# background_hue_50='000000' to set the background pallet

# replace primary above with accent or background if you wish to alter those pallets


# foreground properties
foreground_normal="ffffffde"
foreground_focus="e4e4e4"
foreground_urgent="CC9393"
foreground_critical="232323"

# background properties
background_focus="5a5a5a"
background_urgent="3F3F3F"
# override these for the blurred background properties
# these are symlinked to background_hue_800 and background_hue_900
background_800="000000"
background_900="000000"
background_transparent="66"

# background effect in notification panel
background_modal="ffffff20"
background_modal_title="ffffff30"

# border properties
border_marked="CC9393"

# tooltip properties eg notifications
tooltip_bg="232323"

# taglist properties (the workspaces)
taglist_occupied="ffffff"
```

Each seperate item in the file denotes another color of the window manager.
Play around with it.
To reload the configuration use `mod4+Ctrl+r`

### Icons

The same configuration can be done for icons. What do we mean about icons?
Well you can change the logo, workspace icons, Even icons in the topbar.

All you need to do is edit `.config/tos/icons.conf`

Below is an example file

```bash
# alter tag lists here. The path should be absolute (icons of your workspace)
browser="/etc/xdg/awesome/theme/icons/themes/tos/firefox.svg"
code="/etc/xdg/awesome/theme/icons/themes/tos/code.svg"
social="/etc/xdg/awesome/theme/icons/themes/tos/forum.svg"
folder="/etc/xdg/awesome/theme/icons/themes/tos/folder.svg"
music="/etc/xdg/awesome/theme/icons/themes/tos/music.svg"
game="/etc/xdg/awesome/theme/icons/themes/tos/google-controller.svg"
lab="/etc/xdg/awesome/theme/icons/themes/tos/flask.svg"
terminal="/etc/xdg/awesome/theme/icons/themes/tos/terminal.svg"
art="/etc/xdg/awesome/theme/icons/themes/tos/art.svg"


# general icons used by the UI can be changed here
menu="/etc/xdg/awesome/theme/icons/themes/tos/menu.svg"
logo="/etc/xdg/awesome/theme/icons/themes/tos/logo.svg"
close="/etc/xdg/awesome/theme/icons/close.svg"
logout="/etc/xdg/awesome/theme/icons/logout.svg"
sleep="/etc/xdg/awesome/theme/icons/power-sleep.svg"
power="/etc/xdg/awesome/theme/icons/power.svg"
lock="/etc/xdg/awesome/theme/icons/lock.svg"
restart="/etc/xdg/awesome/theme/icons/restart.svg"
search="/etc/xdg/awesome/theme/icons/magnify.svg"
monitor="/etc/xdg/awesome/theme/icons/laptop.svg"
wifi="/etc/xdg/awesome/theme/icons/wifi.svg"
volume="/etc/xdg/awesome/theme/icons/volume-high.svg"
muted="/etc/xdg/awesome/theme/icons/volume-mute.svg"
brightness="/etc/xdg/awesome/theme/icons/brightness-7.svg"
chart="/etc/xdg/awesome/theme/icons/chart-areaspline.svg"
memory="/etc/xdg/awesome/theme/icons/memory.svg"
harddisk="/etc/xdg/awesome/theme/icons/harddisk.svg"
thermometer="/etc/xdg/awesome/theme/icons/thermometer.svg"
plus="/etc/xdg/awesome/theme/icons/plus.svg"
network="/etc/xdg/awesome/theme/icons/network.svg"
upload="/etc/xdg/awesome/theme/icons/upload.svg"
download="/etc/xdg/awesome/theme/icons/download.svg"
```
> All paths must be absolute!
{.is-warning}

### Tags

The next file is called `.config/tos/tags.conf`
It contains application behaviour such as where should an application launch. And what is the windowing style of a specific window.

Here is an example config file

```bash
# this file is used to automatically move applications to a certain screen when they launch
# You can also specify the style of layout you want

# If your application is not launching in the correct window you should try to following
# Each application has a class name you can find it by using this command : xprop | grep 'WM_CLASS'
# After launching that tool press on your application to get its name

# tell the style of layout
# By default dwindle is used
# dwindle or 0 = spiral format
# floating or 1 = normal Desktop environment behaviour
# tile or 2 = tiling layout
# max or 3 = fullscreen layout

# examples
#screen_1=max
#screen_2=1
#screen_3=tile
#screen_5=floating

screen_8=floating

# TOS put all the browser on the first tag by default
screen_1_1 = "firefoxdeveloperedition"
screen_1_2 = "Google-chrome"
screen_1_3 = "Brave-browser"
screen_1_4 = "Chromium"
# TODO: Add vivaldi and opera



# TOS puts all the code editors on the third window
screen_3_1 = "Code"
screen_3_2 = "code-insiders"
screen_3_3 = "Android-studio"
screen_3_4 = "jetbrains-studio" # the full android studio editor is called as such

# file managers go to the fourth
screen_4_1 = "Nemo"

# Media players go to the fifth
screen_5_1 = "Vlc"
screen_5_2 = "Spotify" # spotify doesn't work since they incorrectly set there class name during startup

# Graphical editors go to the seventh
screen_7_1 = "Inkscape"
screen_7_2 = "Gimp"
```

### Floating

Last configuration file of the window manager is for floating applications.

Some applications really don't launch very well with tiling window managers.
This config aims to help you with that kind of issues.

You can specify which application should always launch in floating mode.

The config name is `.config/tos/floating.conf`

Example content:

```bash
# add a window to automatically float in this config

# If your application is not launching in the correct window you should try to following
# Each application has a class name you can find it by using this command : xprop | grep 'WM_CLASS'
# After launching that tool press on your application to get its name


float_1 = "Inkscape"
float_2 = "Gimp-2.10"
```

### Autostart Applications
The last configuration 'file' is called the autostarted.
It is basically a directory that you can create. Each shell script that is in there will be executed on start.

The location of said directory is `.config/tos/autostart/`

Here is an example file inside of that directory

```bash
#!/bin/bash

xinput set-prop "ETPS/2 Elantech TrackPoint" "libinput Accel Speed" -0.5
```

> All scripts that you want to execute on launch need to have executable permissions
> All scripts must end with .sh to be executed
> The rest of the name you can choose by yourself

## TOS Daemon

The tos daemon will always be launched during boot and manages basic system states.

If you want to edit this make sure `tos-tools` is installed.

Below is a list of what the daemon can do.
To interface with the daemon you need to use the `tos` cli tool.

### Bluetooth

If you wish to enable/disable bluetooth you can use the following command
`tos bluetooth set on`
`tos bluetooth set off`

Alternativly you can edit `.config/tos/theme` and change the line containing `bluetooth=.*` to either `true` or `false`

### theme

You can also change the tos theme.
To see the effect that the theme has try and execute `tos theme set /path/to/picture`
If you wish to enable random theme selection you can add multiple pictures using `tos theme add /path/to/picture`

But don't forget to enable random mode
`tos theme random on` or `tos theme random off`

Alternativly you can enable/disable random mode in `.config/tos/theme`
You should edit
```bash
off # to on
time=1800 # change 1800 to the seconds you wish to cycle the random themes
```
Lastly there is an option in the config called `full=off` or `full=on`
This changes the behaviour of the theme.

If `full=on` then the background image will change and colors in applications will also change (eg terminal)

IF `full=off` then only the background image will change

## System Updater

The last package you can edit is the system-updater.
This updater is used to correctly update your system.

For example it will install all packages needed to function as expected. Even if you delete them by hand.
If you wish to change this behaviour than you can configure it.

### Packages

The config file is located in `/etc/system-updater.conf`
just add a line `exclude = <YourPackagName>` if you don't want that package installed.

Here is a list of all packages that currently will be installed each time you invoke the `system-updater` command.

```bash
awesome-tos
light
archlinux-keyring
st-tos
skel
bluez
compton
feh
python-pip
rofi
zathura
arandr
playerctl
nm-connection-editor
xdotool
xcursor-human
mcmojave-circle-icon-theme-git
breeze-gtk
python-pywal
xf86-input-mouse
xf86-video-intel
pavucontrol
yay
base-devel
glances
lsd
ccat
neofetch
networkmanager
netctl
openssh
pulseaudio
alsa-utils
bluez-utils
chafa
wget
xcb-util-keysyms
xcb-util-wm
libev
yajl
startup-notification
pango
perl
xcb-util-cursor
libxkbcommon-x11
xcb-util-xrm
zsh
xorg-fonts-type1
xorg-server
xorg-xbacklight
xorg-xclipboard
xorg-xinit
xorg-xprop
xorg-xinput
xorg-xrandr
xclip
xorg-xinput
ttf-bitstream-vera
ttf-computer-modern-fonts
ttf-fira-code
ttf-fira-mono
ttf-inconsolata
ttf-liberation
ttf-linux-libertine
ttf-monoid
ttf-symbola
sdl_ttf
siji-git
noto-fonts
font-bh-ttf
bdf-unifont
ttf-dejavu
python-dbus
xorg
tlp
tos-grub-theme
xf86-input-synaptics
base
compton-tryone-tos
materia-gtk-theme
blueman
xfce4-settings
xfce4-power-manager
acpi
redshift
maim
linux-firmware
libarchive
nemo
system-updater
pacman-contrib
libinput-gestures-tos
sddm-theme-tos-git
amd-ucode
intel-ucode
alsa-lib
alsa-plugins
alsa-utils
```

For a more up to date list look [here](https://github.com/ODEX-TOS/system-updater/blob/master/packages)

## Environment variables

Some aspects of the operating sytem can be changed by using environment variables.
If you want to change/alter variables please open the `~/.xprofile` file and adjust them there.

Here is a list of variables that `tos` takes into consideration.

- `EDITOR` - the default text editor to open defaults to `vim`
- `BROWSER` - the browser to use when opening files/url's default to `firefox-developer-edition`
- `TERMINAL` - the default terminal to use defaults to `st`