---
title: Download
description: Easily setup/run TOS
published: true
date: 2020-07-05T13:28:42.054Z
tags: tos, getting started, basics, iso, user, download, live iso
editor: markdown
---

# Download TOS
This page will help you get started with booting into TOS. You'll be up and running in a jiffy!

## Prepare Disk
Download the latest version of TOS GNU/Linux [here](https://tos.odex.be/downloads).

> Note that the image used here is the awesome edition. This edition is the only fully supported version.
{.is-warning}

### Verify iso
Download the corresponding gpg/sha256 files to verify if your iso is legit.
The `sha256` file verifies that no data has gone corrupt during the download.
However it doesn't protect you against tampering.
The `gpg` file verifies that the author of the iso is in fact `odex`
> Try downloading the gpg key with wget, firefox/chrome/curl all tamper with the binary data in the gpg file causing a bad signature
{.is-warning}

#### Verify the checksum

Download the `*.sha256` file and run the following command

```bash
sha256sum -c /path/to/*.sha256
```

> The above command must be ran in the same directory as where your iso is located.
> If you want to run it from a different directory open the `.sha256` file and edit the path there
{.is-warning}

#### Verify the gpg file

First you must have the `tom@odex.be` key inside your gpg keyring.
If you don't have it already download the public key [here](https://repo.odex.be/public.gpg)
And run the following command to import it into your keyring.

```bash
gpg --import /path/to/public.gpg
```

Verify that the key is imported

```bash
gpg --list-keys # tom@odex.be should be in the output
```

Output should look like the following
```bash
pub   rsa4096 2019-08-22 [SC]
      909405E97E931D926D9190321E30222AEBBEC918
uid           [ultimate] Tom Meyers <tom@odex.be>
sub   rsa4096 2019-08-22 [E]
```

Verify that the iso is legit

```bash
# Download the gpg file
# Note: wget seems to be the only utility
# to correctly download the signature
# most other utilities like a webbrowser or curl
# somehow tamper with the file which results in a bad signature
wget https://repo.odex.be/toslive-awesome.iso.gpg -O /path/to/signature/of/iso.gpg

# verify the actual file using gpg
gpg --verify /path/to/signature/of/iso.gpg /path/to/iso
```

If you just want a yes or no as output use the following command

```bash
gpg --verify /path/to/signature/of/iso.gpg /path/to/iso &>/dev/null && echo "Valid" || echo "Invalid"
```

If the above is invalid it means someone is messing with either our servers or with your network connection/dns.

> Try downloading the gpg key with wget, firefox/chrome/curl all tamper with the binary data in the gpg file causing a bad signature
{.is-warning}

### Find your usb stick

If the above is ok then you can continue with flashing your usb stick (or cd)

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
{.is-info}

```bash
dd if=~/Downloads/toslive-awesome.iso of=/dev/sdb status=progress # where sdb is the name of your stick
```

## Booting into TOS GNU/Linux

Reboot your system and enter the bios. If you don't know how to use the bios or enter it. Try to search for help online since the procedure depends on the manufacturer of your device.

> Pressing F12 or F2 when booting is a common way to enter the bios. It doesn't always work. If it doesn't you will need to search online how to enter your bios (depends on the manufacturer)
{.is-info}

When you are booting into tos you will see a bootloader. Just select the first option (boot into TOS).
This will start the boot up sequence. After a few seconds/minutes you will be presented with the TOS environment.

> Depending on the type of usb port/key the boot up sequence can take longer. Since older usb's are rather slow.
{.is-info}

When using the awesome edition you will be presented with a tutorial. It is recommended that you follow this tutorial to the letter. For more information about our awesome edition please visit the navigation section.

> Open up a terminal (mod+Enter) and type in tos c. This will start the installer
{.is-warning}

Feel free to play around in the Live environment.
After you feel comfortable you can start the installer using `tos c` or `tos calamares` inside of a terminal.

Have fun!

> Visit [here](/Usage/navigate) if you want to learn more about how you should navigate the environment.
{.is-info}
