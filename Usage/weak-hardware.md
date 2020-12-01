---
title: Weak Hardware
description: Alter TOS to be used on weak hardware (low cpu and low memory footprint)
published: true
date: 2020-12-01T11:01:09.175Z
tags: tos, tde, hardware, configuration
editor: markdown
---

# Weak Hardware
This page will cover configuration options to reduce the cpu and memory footprint in order to run on lower end hardware.


## TDE

### config options
First of all `TDE` provides an option called `weak_hardware` it disables a lot of features to reduce the memory footprint of `TDE`
The option can be found in the file `~/.config/tos/general.conf` and should look like this:
```
# Set to one to enable weak hardware mode
weak_hardware="1"
```

Afterwards reload the desktop `Mod4 + Ctrl + Shift` or restart the computer.

> Note that you can also edit this option in the settings app under the general tab
{.is-info}

> In our test environment this reduced memory consumption by over `20MB`
{.is-info}

### Wallpaper
By default tde only allows setting images as the wallpaper.
However they do consume up to a few megabytes to display.
By setting the wallpaper to a solid color you are able to safe memory.
First choose your color (in hex, where `#000000` means black and `#FFFFFF` means white)
Set the wallpaper as such:
```bash
xsetroot -solid "#000000"
```
If you are in a tty use the following command instead
```bash
DISPLAY=:0 xsetroot -solid "#fff"
```

These settings will only persist for this session.
To keep the changes create the following file:
`~/.config/tos/autostart/wallpaper.sh`
with the content:
```bash
xsetroot -solid "#fff"
```

Don't forget to make it executable or it won't work:
```bash
chmod +x ~/.config/tos/autostart/wallpaper.sh
```

> In our test environment this reduced memory consumption by over `2MB`
{.is-info}

## Services
Another issue could be that to much services/daemons are running
To get a list of active daemons execute the following command
```
systemctl list-units --type=service --state=running
```
Commong service files that need to be running are:
* accounts daemon
* avaho-daemon
* dbus
* irqbalance
* lightdm
* NetworkManager
* polkit
* rtkit daemon
* systemd-logind
* systemd-timesyncd
* systemd-udevd
* user@xxxx.service
* wpa_supplicant

All other services can be disabled and should be disabled if not used:
The command to execute is as such
```bash
# stop running the service
systemctl stop <unit>
# don't run it after a reboot
systemctl disable <unit>
```

> In our test environment, we reduced memory consumption by over `50MB`
{.is-info}

## Processes
Lastly you can check if some process doesn't need to be running
check the output of `ps aux` to find processes that don't need to be running. You can 'stop' or 'kill' them using the following command
```bash
kill <pid>
# example, kill process with pid 1022
kill 1022
```

## Results

In our system low hardware system with 1Gb of ram and 2 cpu cores we managed to reduce the system load from `340MB` to `200MB` and cpu usage of around `2%` to around `1%`


