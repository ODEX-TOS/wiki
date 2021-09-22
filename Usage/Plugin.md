---
title: Plugins
description: Plugins that are written for TDE
published: true
date: 2021-09-22T10:45:19.522Z
tags: tde, plugins, lua, code, customization
editor: markdown
dateCreated: 2020-07-04T16:47:09.291Z
---

# Plugins
Since version `0.6.0` TDE now includes custom plugins. 
These can be enabled/disabled using the settings -> plugins (After version `0.9.0`)
![image.png](/image.png)

> Note: previous version need to edit `~/.config/tos/plugins.conf` 

Plugins can be stored in 2 locations:
- `~/.config/tde/` - User plugins
- `/etc/tde/plugins` - System-wide plugins
Each plugin should be inside its own directory.
e.g. `~/.config/tde/my-plugin-name/*`

`TOS` provides some plugins by default, check `/etc/tde/plugins/*` for example plugins

> To get an in depth api overview visit the following
<a href="https://tos.odex.be/docs/index.html">link</a>
{.is-info}

## Plugin structure

The directory strucuture must at least look like this:
```txt
.config/
├─ tde/
│  ├─ Plugin name/
│  │  ├─ init.lua
│  │  ├─ metadata.json
```

- `init.lua` is the code entrypoint of your plugin
- `metadata.json` contains some extra data about your plugin such as version, icon, description etc

### init.lua

The most basic plugin would be a module that prints to the log file, the `init.lua` would look like this:

```lua
print("Hello World from my plugin")
```

### metadata.json

For the init.lua above (Which is a module (see below for more info)) this is the full metadata.json file

```json
{
        "type: "module",
        "icon": "/path/to/your/icon",
        "version": "v1.0.0",
        "description": "A simple hello world plugin",
        "description_en": "A simple hello world plugin",
}
```

- `type` - The type of plugin to be loaded (`module`, `notification`, `settings`, `topbar`, `prompt`)
- `icon` - A path to the icon, can be either absolute or relative
- `version` - The version of your plugin, using semver
- `description` - A description of what your plugin does
- `translation` - By suffixing your description with the languagecode eg `description_en` for english, `description_es` for spanish, `description_fr` for french etc you can translate the description


Now go to the settings -> plugins and you should be able to run your plugin.

Before activating open up a terminal and type in `tail -f ~/.cache/tde/stdout.log` in order to see the plugin print

## Type
Our plugin system separates plugins into two categories.

1. `Modules`:
 These plugins are simple scripts written in `lua` that are ran in the background under the same process as widgets (thus they can manipulate widgets and other things). Users usually don't see these plugins working. Examples are: low battery notifier, keybindings, settings modifiers, file watchers and many more. You can compair then with unix daemons.

2. `Widgets`: 
 Next you have widgets, These are the plugins you see every day. They are the GUI representation of a certain object. You can see them all over the place. Starting from the clock, package updater, wifi widget, workspaces, settings app and more.
 
When writting a plugin you must take into consideration which one to use.
Everything is allowed in a Module, however when writing a Widget you must inherit `wibox.widget` and return such an object to be consumed by `TDE`

> Note: `wibox.widget` is the widgeting system in tde

## widgets

Plugins widgets can be placed in multiple locations around the user interface.
In the configuration file you can specify where the widget should be placed.
Here are the current widget locations.

1. `topbar` - The widget will be placed in the topbar on the right side next to all the buttons
2. `notification` - The widget will be put in the notification center under the widget tab
3. `settings` - The widget will be placed at the bottom of the settings app (left menu)
4. `prompt` - The widget will provide autocomplete for the prompt (Mod + F2)

### topbar

Here is an example of the most basic topbar plugin.

> A topbar plugin requires returning a `wibox.widget` instance


```lua
-- required library
local wibox = require("wibox")

return wibox.widget.textbox("This is an example topbar plugin")
```

The `metadata.json` file should look like this:
```json
{
    "type": "topbar",
}
```

### notification
Here is an example of the most basic notification plugin.

> A notification plugin requires returning a `wibox.widget` instance

```lua
-- required library
local card = require('lib-widget.card')()
local wibox = require("wibox")

card.update_body(
	wibox.widget.textbox("This is an example notification plugin")
  )

return card
```

The `metadata.json` file should look like this:
```json
{
    "type": "notification",
}
```

### settings
Here is an example of the most basic topbar plugin.

> A settings plugin requires returning a table in the following format:
> ```lua
>{
>  icon = "/path/to/your/icon",
>  name = "the name of your settings application",
>  -- the settings menu expects a wibox
>  widget = create_widget() -- should return a wibox.widget
>}
> ```
> The `icon` is used in the navigation tab to show your settings (It can be dynamically generated)
> The `name` is used in the settings tab
> The `widget` is the setting page element


```lua
-- required library
local wibox = require("wibox")
local icons = require("theme.icons")

return {
  icon = icons.warning,
  name = "example settings",
  -- the settings menu expects a wibox
  widget = wibox.widget.textbox("This is an example settings plugin")
}
```

The `metadata.json` file should look like this:
```json
{
    "type": "settings",
}
```

### prompt
Here is an example of the most basic prompt plugin.

> A prompt plugin requires returning a table in the following format:
> ```lua
>{
>  get_completion = function() end,
>  perform_action = function() end,
>  name = "The name of this plugin"
>}
> ```
> The `get_completion` function is used to generated the prompt list for the user
> The `perform_action` function is called when the user selects an option returned from the `get_completion` function
> The `name` is the name of this specific prompt resolver

```lua
-- used to fetch icons
local icons = require("theme.icons")

-- This function should return a completion list
-- This list will be displayed in the prompt
-- The supplied query is the search string the user typed in
-- The function should return a list of elements with the following properties:
-- icon -> a string pointing to an image on the filesystem - or a cairo surface
-- text -> a string showing the text to the end user in the prompt
-- payload -> a custom object that will be passed to the `perform_action` function when this item is selected
local function get_completions(query)
    return {
        {
            icon = icons.unknown,
            text = "Plugin completion for: " .. tostring(query),
            payload = query,
            __score = math.huge -- optional argument to override the place in the prompt to appear
        }
    }
end

-- This function will be called when an item from this plugin is selected in the prompt
-- The payload parameter is the payload you supplied in the `get_completions` function
-- You can perform the action here that the end user requested, for example open a webpage, login via ssh, open an application etc
local function perform_action(payload)
    print("Prompt example plugin got payload: " .. payload)
end


return {
    get_completion = get_completions,
    perform_action = perform_action,

    -- Don't forget to change this name to the name of your plugin
    name = "example plugin"
}
```

The `metadata.json` file should look like this:
```json
{
    "type": "prompt",
}
```

# API

As stated before the language in which plugins are written is `lua`
Here is a list of useful websites helping you learn the language:
1. [Getting started](https://www.lua.org/start.html)
2. [Tutorial](https://www.tutorialspoint.com/lua/index.htm)

Since `TDE` is build ontop of `awesomeWM` you can use their [api](https://awesomewm.org/apidoc/)
A few things you should know about their api is the `wibox` and `wibox.widget` classes (used for widgets).
You should also need to know `naughty` if you want to use notifications.
`beautiful` is used as the theming api and `lib-tde.signals` to communicate to other parts of `TDE`.

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
local delayed_timer = require("lib-tde.function.delayed-timer")

local timer = delayed_timer(
-- run the timeout every 10 seconds
	10,
  -- the callback function to execute
  function()
  	print(os.date():sub(9))
  end,
  -- start the timer after 0 seconds (instantly)
  0
)
```

## signals

Sometimes you want to receive event from one widget or module to another.
For example when something needs to be updated like when a new screen has been plugged in.


```lua
local signal_handle = require("lib-tde.signals")
-- This will be called when the volume slider get updated
signal_handle.connect_volume(
  function(value)
    print("The system volume changed to: " .. tostring(value) .. "%")
  end
)

-- this will emit a signal that can be catched by anyone listning
-- It tells the listners that the volume is 100%
signal_handle.emit_volume(100)
```

## translations
Translations are a big part of software.
Every decently sized plugin needs translations to grow.
TDE provides a built in translation library it is exposed as a global in lua `i18n`

A plugin can supply it's own set of translations using the following constructs

```lua
-- returns the current language as a string, eg 'en', 'nl', 'fr', 'es' etc
local language = i18n.getLanguage()
local translation = {}

if language == "en" then
 translation["hello"] = "hello"
 translation["good day"] = "good day"
 i18n.custom_translations(translation)
elseif language == "nl" then
 translation["hello"] = "hallo"
 translation["good day"] = "goeiedag"
 i18n.custom_translations(translation)
end

-- now you can use the translations
print(i18n.translate("hello"))
print(i18n.translate("good day"))
```

> To get an example of how plugins work look [here](https://github.com/ODEX-TOS/tos-desktop-environment/tree/release/plugins)
{.is-info}

> To get an in depth api overview visit the following
<a href="https://tos.odex.be/docs/index.html">link</a>
{.is-info}



