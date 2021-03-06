---
title: Setting Up WI-FI
description: Small tutorial on all ways you can configure wifi
published: true
date: 2020-06-21T10:57:45.774Z
tags: tutorial, help, user, wifi
---

# Setting up wifi
Starting up a network connection can happen in one of 2 ways on tos. The GUI and terminal will both be explained here.

## GUI Connection to your wifi network

Connecting to a network should be pretty simple. This is why we provide an easy to use interface for connecting to wireless networks. All you have to do is open the action center (upper left side on the tos logo.

![action-center-location.png](/images/action-center/action-center-location.png)

Once opened up you should see the following.

![action-center.png](/images/action-center/action-center.png)

Press the connect to wireless network button to start the network connection wizard.
Select the desired SSID to connect to and optionally enter the password.

> We only support WEP/WPA and WPA2. Enterprise secured networks currently don't work with this option. You need to resort to the terminal to establish a network connection
{.is-warning}

# Terminal
This one is also pretty simple. All you need to know is your SSID and your password. Open up a terminal and type in the following

```bash
tos network list # get a list of all detected networks
tos network connect <ssid> # connect to a network called <ssid>
```

If you are trying to connect to enterprise network or the connection type is not supported try the following

```bash
nmtui # open a TUI (terminal user interface)
```
