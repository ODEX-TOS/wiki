---
title: Plugins
description: Plugins that are written for TDE
published: true
date: 2020-07-05T13:26:55.441Z
tags: tde, plugins, lua, code, customization
editor: markdown
---

# Plugins
Since version `0.6.0` TDE now includes custom plugins. These can be enabled/disabled using `~/.config/tos/plugins.conf` and are stored in `~/.config/tde/`
Each plugin should be inside its own directory.

`TOS` provides some plugins by default if `skel` is installed.
You can visit the example plugins under `/etc/skel/.config/tde`
This file will describe how such plugins work.

First you must understand that their are 2 different kind of plugins.

## Type

1. `Modules`:
 These plugins are simple scripts written in `lua` that are ran in the background under the same process as widgets (thus they can manipulate widgets and more). Users usually don't see these plugins working. Examples are: low battery notifier, keybindings, settings modifiers, file watchers and many more. You can compair then with daemons.
2. `Widgets`: 
 Next you have widgets, These are the plugins you see every day. They are the GUI representation of a certain object. You can see them all over the place. Starting from the clock, package updater, wifi widget, workspaces, settings app and more.
 
When writting a plugin you must take into consideration which one to use.
Everything is allowed in a Module, however when writing a Widget you must inherit `wibox` and return such an object to be consumed by `TDE`

## widgets

Plugins widgets can be placed in multiple locations around the user interface.
In the configuration file you can specify where the widget should be placed.
Here are the current widget locations.

1. `settings` - The widget will be placed at the bottom of the settings app (left menu)
2. `topbar` - The widget will be placed in the topbar on the right side next to all the buttons
3. `notification center` - The widget will be put in the notification center under the widget tab

# API

As stated before the language in which plugins are written is `lua`
Here is a list of useful websites helping you learn the language:
1. [Getting started](https://www.lua.org/start.html)
2. [Tutorial](https://www.tutorialspoint.com/lua/index.htm)

Since `TDE` is build ontop of `awesomeWM` you can use their [api](https://awesomewm.org/apidoc/)
A few things you should know about their api is the `wibox` and `wibox.widget` classes (used for widgets).
You should also need to know `naughty` if you want to use notifications.
`beautiful` is used as the theming api.

Below we will cover a few important subjects to get you started

## sizing

When working with widgets it is important that the sizing is always correct.
`TDE` adherese to the Xorg dpi spec. This means that application can scale in size depending on what the user has set as their `dpi`.
This is usually used when a screen is massive (eg 4k or 8k) the applications and text become so tiny that people can't read it anymore. Thus dpi was introduced as a scalar.

When specifying sizes in lua you should do the following
```lua
-- import dpi to calculate the correct pixel amount depending on what the end user wants
local dpi = require('beautiful').xresources.apply_dpi
-- instead of working with hardcoded pixel values dpi will calculate the size of every margin
wibox.container.margin(widget, dpi(14), dpi(14), dpi(6), dpi(6))

-- NEVER WORK WITH RAW PIXEL DATA!
wibox.container.margin(widget, 14, 14, 6, 6)
```

## theming

`TDE` has a config file to specify the color scheme the end user is using.
All those colors can be accessed using `beautiful` here is an example of background and foreground colors.

```lua
-- holds all theming values
local beautiful = require('beautiful')
beautiful.bg_normal -- normal background color
beautiful.fg_normal -- normal foreground color
beautiful.bg_modal -- background of a card (look in the notification center)
beautiful.bg_modal_title -- background of a title of a card
beautiful.font -- default font
beautiful.title_font -- default font for titles

-- color pallets
beautiful.primary -- primary color pallet
beautiful.accent -- accent color pallet
beautiful.background -- background color pallet
beautiful.primary.hue_800 -- specific color hue
beautiful.primary.hue_700 -- lighter version of the background

-- possible hues are
-- 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, A100, A200, A400, A700

-- USAGE IN A WIDGET
local separator = wibox.widget {
  orientation = 'horizontal',
  forced_height = 1,
  span_ratio = 1.0,
  opacity = 0.90,
  color = beautiful.bg_modal,
  widget = wibox.widget.separator
}
```

### icons

By default it is recommended to use svg files as icons.
The reason behind is that scaling will always look good.
Scaling png or jpeg files will always introduce artifacts.
When using svg icons you need to be aware about the color of the svg.
For example when someone is using a light theme the icons should be dark because otherwise the contrast would be to small.
When using a dark theme the icons should be light.
For this reason we introduced a function that takes an svg path as argument and depending on the theme it will try to return either a light or dark variant.

```lua
-- this is a function that will convert your svg path to eiter dark or light
local svgColor = require('theme.icons.dark-light')


-- the below function call will return /home/user/.config/tde/icon-example/icons/globe.svg if the theme is light
-- otherwise it will return /home/user/.config/tde/icon-example/icons/globe.dark.svg
svgColor("/home/user/.config/tde/icon-example/icons/globe.svg")
```

> Make sure a dark variant of you icon exists otherwise light theming is not supported for your plugin.
> For example if your normal icon is called `icon.svg` than create a second icon named `icon.dark.svg`
{.is-info}

{.is-warning}

## notification

Notifications for `TDE` are rather easy. All you have to do is build and notifcation object and it will automatically be shown to the user.

```lua
-- import the notification library
local naughty = require("naughty")

-- generate a notification
naughty.notify({ text = "My First Notification",
                 icon = icon, -- an optional icon
                 timeout = 5, -- notification disappears after 5 seconds
                 screen = mouse.screen
                 })
```

You can even do something when the notification is destroyed

```lua
local function naughty_destroy_callback()
	print("User destroyed my first notification")
end

local notification = naughty.notify({ text = "My First Notification",
                                icon = icon,
                                timeout = 4,
                                screen = mouse.screen,
                                })
notification:connect_signal("destroyed", naughty_destroy_callback)
```

Optionally you can set the title

```lua
-- import the notification library
local naughty = require("naughty")

-- generate a notification
naughty.notify({ text = "My First Notification",
								 title = "My first plugin",
                 icon = icon, -- an optional icon
                 timeout = 5, -- notification disappears after 5 seconds
                 screen = mouse.screen
                 })
```

## General widgets

## Shell scripts
Sometimes you need to interface with the system in a more generic way.
You can execute shell scripts and even read the output in lua

```lua
-- import the generic api
local awful = require("awful")

-- run a shell script
awful.spawn("sh /path/to/my/shell/script.sh")

-- example where you read out the shell output
awful.spawn.easy_async_with_shell("seq 1 10", function(out)
	print("Shell script finished with output:")
  print(out)
end)
```

## timeout

Sometimes you need to do something every x seconds an example would be to get the time and update the clock every second
Below is such an example

```lua
-- import the timeout library
local gears = require("gears")

local timer = gears.timer {
	-- run this every minute
  timeout = 60,
  -- start by itself (otherwise run timer:start() to start and timer:stop() to stop)
  autostart = true,
  -- this function get ran every minute
  callback = function ()
    print(os.date():sub(9))
  end
}
```

## signals

Sometimes you want to receive event from one widget or module to another.
For example when something needs to be updated like when a new screen has been plugged in.


```lua
-- This will be called when the volume slider get updated
awesome.connect_signal(
	'widget::volume',
  function(value)
    print("Updated volume slider")
  end
)

-- this will emit a signal that can be catched by anyone listning
awesome.emit_signal("widget::volume")
```

> To get an example of how plugins work look [here](https://github.com/ODEX-TOS/dotfiles/tree/master/tde)
{.is-info}

> To get an in depth api overview make sure `awesome-tos` is installed and visit the following [link](file:///usr/share/doc/tde/doc/index.html)
{.is-info}



