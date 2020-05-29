---
title: Basic Usage
description: How to work inside of TOS
published: true
date: 2020-03-17T15:00:02.492Z
tags: basics, getting started, tutorial, help, flow
---

# Basic Usage

This guide is for absolute beginners.

We will cover the basics of most functions you need to be aware about.

## Desktop

Lets look at the desktop

![desktop.png](/images/basic-setup/desktop.png)

By design you can't place icons/application in this desktop.
It is purely aesthetic.
You can however use the right mouse button to show a menu containing your application.

It looks like this ![apps.png](/images/basic-setup/apps.png)

Their isn't much to see on the desktop except for the top bar

![topbar.png](/images/basic-setup/topbar.png)

Feel free to see what each `widget` in the top bar does.
Most should be self explanatory.


## Centers

### Action Center

![desktop.png](/images/basic-setup/desktop.png)

Do you see the `tos` logo at the top left?

It looks like this

![tos_logo.png](/branding/tos_logo.png)

If you click on it you will get the action center.

It looks like the following

![action-center-rofi.png](/images/basic-setup/action-center-rofi.png)

It informs you about the most basic information about your system.

You can do the following:

- Read basics stats like disk usage, temperature
- connect to a wireless network
- Change volume/brightness
- Enable/disable settings like wifi/bluetooth

But as you can see there are 2 more options:

- Change Application Scaling
- OLED Brightness mode

The `Change Application Scaling` will make icons look either bigger or smaller.

Try the option to see what happens. If you don't like it you can always set it back to 1.

The second option is `OLED Brightness mode` you should enable it only when your screen is an oled screen or when the brightness slider is not working as expected.

### Notification Center

Lets look at the Notification Center.

![desktop.png](/images/basic-setup/desktop.png)

Do you see the top most right icon?

Clicking on it or using the keyboard shortcut `mod4+X` will show the following

![notif-main.png](/images/basic-setup/notif-main.png)

This should show all latest notification you have received.

If you don't want notification you can toggle `Do Not Disturb`

There is also a button called `Widgets` upon clicking you are greeted with the following.

![notif-widgeet.png](/images/basic-setup/notif-widgeet.png)

It currently contains:

- System/user information
- Social media shortcuts
- Local Weather Information
- Calculator

Try and test them out!

## keyboard

Since `tos` is keyboard driven, we recommend you get familiar with its keyboard layout.

If you want to get a more specific description read the following [link](/user/navigate)

All you need to know for now is that most shortcuts use `mod4`

The key usually looks like this:

![windows.jpg](/images/basic-setup/windows.jpg)

### application launcher

To get a list where you can launch a specific application run

`Mod4+D` (The Windows Key together with the D Key)

You should get something like this

![rofi-popup.png](/images/basic-setup/rofi-popup.png)

Start typing the name of your application.

By pressing enter you launch the application.

If you want to quit the app simply press `Mod4+Q`

### terminal

When using `tos` it is important that you can use the terminal.

To launch a terminal press `Mod4+Enter`

Here is an easy tutorial about terminals and shells
[link](https://towardsdatascience.com/basics-of-bash-for-beginners-92e53a4c117a)

Bash is the most used shell. However we use `zsh` as it provides a bit more comfort.

## updating

Finally you have to keep your system up to date.

> unlike windows we offer the choice when you want to update your system (we don't enforce it, but we do recommend it)
{.is-info}

For a detailed describtion to keep your system up to date read
- [Update and install software](/user/update)
- [Cleanup and routine maintanance](/start/maintain)

All you should know to get started is to open a terminal and type `system-updater`
