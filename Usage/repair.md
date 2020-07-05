---
title: System Repair
description: What to do when your system is utterly broken
published: true
date: 2020-07-05T13:27:45.713Z
tags: tutorial, help, user, repair, fix, broken
editor: markdown
---

# System Repair

Sometimes your system is utterly broken.
This page is here to help you fix certain things.

> This will cover how to recover broken system
> It requires you to reboot the system in an advanced mode
> Shell knowledge is highly recommend to not further destroy your system
{.is-warning}

## Password recovery

This section describes how to reset the password.
For example when you by accident overrode the root password

### bash init
This will change the initial process from systemd to bash
This way you have a shell (and only a shell) to change system data

1. Set a kernel parameter init=/bin/bash (in the bootloader enter e)
2. Remount the root filesystem as read/write `mount -n -o remount,rw /`
3. Use passwd to change the root password
4. Reboot the system `reboot -f`

### recovery medium

This section requires you to have a `tos` or `arch` usb stick.
We will mount the disk on the liveusb and chroot into to gain access

1. Boot the LiveCD and mount the root partition of your main system.
2. Use the `passwd --root <MOUNT_POINT> <USER_NAME>` command to set the new password (you will not be prompted for an old one).
3. Unmount the root partition.
4. Reboot, and enter your new password. If you cannot remember it, go to step 1.

## Remove broken packages
You could by accident install packages that break your system.
For example removing the linux kernel or system files.
This section will cover how to remove the package that broke your system.

### bash init

> If you still have a working kernel the bash init will still work
>{.is-info}

This section is similar to the password recovery
1. Set a kernel parameter init=/bin/bash (in the bootloader enter e)
2. Remount the root filesystem as read/write `mount -n -o remount,rw /`
3. Remove the unwanted packages `pacman -Rns <Your Packages>`
4. Optionaly install a replacement package `pacman -Syu <Fixed package>`
5. Reboot your system `reboot -f`

### recovery medium

> This section is in case your kernel is broken as well
{.is-info}

1. Boot the LiveCD and mount the root partition of your main system.
2. Use the `arch-chroot <mountpoint>` This will change your root to that of your broken system.
3. Remove the unwanted packages `pacman -Rns <Your Packages>`
4. Optionaly install a replacement package `pacman -Syu <Fixed package>`
5. Exit the chroot `exit`
6. Unmount the root partition.
7. Reboot, if it is broken go to step 1.


## fix system files
This section cover how to fix a system that no longer boots due to a system file you edited

### bash init

> This section only works if the issue is not present in the boot process
> This means you didn't mess with `mkinitcpio.conf`, `fstab`, `grub.conf` or another file dedicated to the boot process
{.is-info}

This section is similar to the password recovery
1. Set a kernel parameter init=/bin/bash (in the bootloader enter e)
2. Remount the root filesystem as read/write `mount -n -o remount,rw /`
3. Edit the system files you broke by using your editor eg `vim <file>`
4. Reboot your system `reboot -f`

### recovery medium

> This section is recommended if the above doesn't work or you messed with the boot process
{.is-info}

1. Boot the LiveCD and mount the root partition of your main system.
2. Use the `arch-chroot <mountpoint>` This will change your root to that of your broken system.
3. Edit the system files you broke by using your editor eg `vim <file>`
4. Exit the chroot `exit`
5. Unmount the root partition.
6. Reboot, if it is broken go to step 1.

## Extra Tips

If updating packages if the issue you can do the following

- Revert to an older package version `pacman -U /var/cache/pacman/pkg/<Older File Version>`
- Installing a new package conflicts with existing files/directories `pacman <package> --overwrite <conflicting file>`
