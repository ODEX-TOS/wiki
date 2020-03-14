---
title: Download
description: Easily setup/run TOS
published: true
date: 2020-03-14T20:48:16.915Z
tags: iso, user, download, tos, live iso, basics, getting started
---

# Download TOS
This page will help you get started with booting into TOS. You'll be up and running in a jiffy!

## Prepare Disk
Download the latest version of TOS GNU/Linux [here](https://tos.odex.be/downloads).

> Note that the image used here is the awesome edition. This edition is the only fully supported version.
{.is-warning}

### Find your usb stick

```bash
lsblk # before inserting your stick
#now insert the usb stick
lsblk # the new entry is your usb stick
```

### Burn the iso to your usb stick

```bash
dd if=~/Downloads/toslive-awesome.iso of=/dev/sdb # where sdb is the name of your stick
```

> Note that the above `dd` command can take a long time. If you wish to see progress use the following command

```bash
dd if=~/Downloads/toslive-awesome.iso of=/dev/sdb status=progress # where sdb is the name of your stick
```

## Booting into TOS GNU/Linux

Reboot your system and enter the bios. If you don't know how to use the bios or enter it. Try to search for help online since the procedure depends on the manufacturer of your device.

> Pressing F12 or F2 when booting is a common way to enter the bios. It doesn't always work. If it doesn't you will need to search online how to enter your bios (depends on the manufacturer)
{.is-info}

When you are booting into tos you will see a bootloader. Just select the first option (boot into TOS).
This will start the bootup sequence. After a few seconds/minutes you will be presented with the TOS environment.

> Depending on the type of usb port/key the bootup sequence can take longer. Since older usb's are rather slow.
{.is-info}

When using the awesome edition you will be presented with a tutorial. It is recommended that you follow this tutorial to the letter. For more information about our awesome edition please visit the navigation section.

> Open up a terminal (mod+Enter) and type in tos c. This will start the installer
{.is-warning}

Feel free to play around in the Live environment.
After you feel comfortable you can start the installer using `tos c` or `tos calamares` inside of a terminal.

Have fun!

> Visit [here](/user/navigate) if you want to learn more about how you should navigate the environment.
{.is-info}
