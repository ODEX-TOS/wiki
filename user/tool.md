---
title: TOS Tool
description: Use the TOS tool to make your life easier
published: true
date: 2020-03-14T20:47:49.244Z
tags: user, tutorial, help, us
---

# TOS TOOL

TOS tool is for when you want to manage your system but keep forgetting all the different tools and commands.

It support a bunch of different things you generally need when managing a system. Tools like

- Screen support
- Bluetooth connections
- Wifi connections
- Volume changing
- SSH management
- Package management
- Theme management
And many more.

TOS solves this by combining most of these tools into one package.

> Note: tos tool only works for arch based distributions (yes you can use it on other distro's)
{.is-info}

Installation is pretty straight forward.
Run these commands and you have TOS tool installed

```bash
pacman -Syu python-pywal git curl xrandr bluez bluez-utils networkmanager
git clone https://github.com/ODEX-TOS/tools.git
cd tools
sudo cp tos /usr/bin/tos
sudo mkdir -p /usr/share/tos-helper
for file in tos-helper/* ; do
      cp "$file" /usr/share/"$file"
done
tos -h
```

Now you have installed tos together with all its dependencies.

If you run

```bash
tos -h
```

You will see its help page. TOS divides its arguments in a few subcategories


## screen

```bash
$ tos screen
```

All the suboptions starting with `screen` are screen related

| option                |                      effect                       |
| --------------------- | :-----------------------------------------------: |
| get                   |            Display screen information             |
| duplicate <in> <out>  |         duplication screen <in> to <out>          |
| toggle <in> <on\|off> |       turn the screen called <in> on or off       |
| reset <in>            |   reset the screen <in> to ist default settings   |
| resolution <in> <res> |    set the screen <in> to the resolution <res>    |
| refresh <in> <rate>   | change the frequentie of screen <in> to <rate> Hz |

## Volume

```bash
tos volume
```

All the suboptions starting with `volume` are volume related

| option        |                              effect                               |
| ------------- | :---------------------------------------------------------------: |
| get           |                Display current volume information                 |
| change <num>  | change the volume by the amount of <num> (can be negative values) |
| set <percent> |              Set the volume to <percent> percentage               |
| toggle        |            Toggle the current volume channel on or off            |

## Bluetooth

```bash
tos bluetooth
```

All the suboptions starting with `-b` are bluetooth related

| option           |                       effect                       |
| ---------------- | :------------------------------------------------: |
| get              |           Display bluetooth information            |
| connect <dev>    |         connect to the device called <dev>         |
| disconnect <dev> |      disconnect from the device called <dev>       |
| list             |          list all known and found devices          |
| list scan        | do the same as above but also scan for new devices |
| full             |      go into an interactive bluetooth prompt       |

## Themes

```bash
tos theme
```

All the suboptions starting with `theme` are theme related

| option             |                  effect                  |
| ------------------ | :--------------------------------------: |
| set <pic>          |        Change the theme to <pic>         |
| time <time>        | set the timeout for theme randomization  |
| add <pic>          |   add a picture to the theme database    |
| delete <pic>       | delete a picture from the theme database |
| list               |         List all known pictures          |
| random <on \| off> |    Turn theme randomization on or off    |

This one needs some explination. How can you change a theme based on a picture?
Well that is actually pretty easy. We scan the picture and generate a color pallet out of it. This color pallet will be applied to every application installed onto the system. This way we have uniform coloring across every component of the system.
Afterwards we set the desktop background to that image as well.

Now you know the basic implementation of our theme but what about randomization?

This one is pretty easy as well. First you must add pictures to a database by doing

```bash
tos theme add <pic>
```

After that you enable theme randomization

```bash
tos theme random on
```

Now every 1000 seconds you will get a random theme from the database.

This will change the theme every x seconds so that you don't get bored of the looks

Now how do you change the time?

```bash
tos theme time 3600 # change to 1 hour
tos t t 10 # change theme every 10 seconds
tos theme time 86400 # change every day
```

As you can see typing in large numbers can be a pain because you can only type in seconds but we have a solution. You can change the time in days, hours, minutes and seconds as well All you have to do is use this syntax

```bash
tos t time 1d-2h-3m-4s
```

As you can guess 1d is one day, 2h is 2 hours etc.
This effectively says that every 1 day and 2 hours 3minutes and 4 seconds the theme will change. But thats not all. You can also do this

```bash
tos theme time 0.5h
```

You can work with fractions of time 0.5 hours is effectively 30m

## Network

```bash
tos network
```

All options starting with `network` are network related

| option                     |                   effect                    |
| -------------------------- | :-----------------------------------------: |
| connect <ssid>             |        connect to wifi by its <ssid>        |
| device                     |        Show all the network devices         |
| list                       |         list all network interfaces         |
| metric <interface> <value> | change the metric of <interface> to <value> |
| restart                    |           List all known pictures           |

## GPG

```bash
tos gpg
```

All options starting with `gpg` are network related

| option                 |                                  effect                                   |
| ---------------------- | :-----------------------------------------------------------------------: |
| info                   |                    Get general information of all keys                    |
| key                    |                            show keys there id                             |
| export <key>           |                     export a key by its id to a file                      |
| import <file>          |                          import a key by a file                           |
| upload <key> <?server> | upload a key to a server. If server is added then it will use that server |
| generate               |                              generate a key                               |
| git                    |                    add a key to your git configuration                    |

## MISC

```bash
tos -*
```

All the suboptions starting with `-\*` are misc options

| option         |                effect                           |
| -------------- | :---------------------------------------------: |
| -iso -g        |        Install TOS graphically(orphaned)        |
| -h             |        print help information                   |
| -c             |         generate crypto keys                    |
| -c <user>@<ip> | copy your public key to that location           |
| -rs            |     Perform a basic system repair               |



