---
title: Tos Build System
description: Simplify your developer life with the tos build system
published: true
date: 2020-03-17T14:02:37.205Z
tags: tutorial, help, dev, simplify
---

# Tos Build System

> You can perfectly develop software for `tos` without the `tbs` aka `tos build system`. However it makes your life a bit easier to install and build tos.
> Below is described how to use `tbs` and how to extend `tbs`
{.is-info}

Tos Build System aka tbs is a piece of software build to make building tos software easier.

- [repo](https://github.com/ODEX-TOS/tos-build-system)

Here is a short list of what it can do now

- Manage dependencies
- Update/build iso images
- Compile the linux kernel
- Manage,update,build your repository
- Upload data/builds to a server

## Usage

`tbs` can be installed using the following command

```bash
tos -Syu tos-build-system
# or on arch with the tos repo
pacman -Syu tos-build-system
```

> Make sure you have enabled to tos repository

Lets look at the main help file to understand `tbs`

```bash
tbs -h
>
usage: tbs [-h] {dependency,image,kernel,launcher,repo,upload} ...

positional arguments:
  {dependency,image,kernel,launcher,repo,upload}

optional arguments:
  -h, --help            show this help message and exit
```

As you can see we have the following options to supply to `tbs`

- dependency
- image
- kernel
- launcher
- repo
- upload

Most subsections speak for themself.

But if you want to know more you can supply the following options

```bash
tbs dependency -h
tbs image -h
tbs kernel -h
tbs launcher -h
tbs repo -h
tbs upload -h
```

Lets cover all these sections

### dependency

Here is the dependency help page

```
usage: tbs dependency [-h] [--repo] [--image] [--kernel] [--upload] [--repo-full] [--all]

optional arguments:
  -h, --help        show this help message and exit
  --repo, -r        Install all repo dependencies
  --image, -i       Install all tos image dependencies
  --kernel, -k      Install all kernel dependencies
  --upload, -u      Install all data upload dependencies
  --repo-full, -rf  Install a full repo dependencies (includes --repo and --kernel)
  --all, -a         Install all dependencies
```

As you can see it manages all the dependencies required to have install for a specific section.

> Each section automatically pulls the needed dependencies. You should normally never need to manually install dependencies using the above commands. However it is useful if you want to debug errors happening in the dependency install procedure.
{.is-warning}

You can choose which dependencies you want to install

For example lets install all dependencies required to build the kernel

```bash
tbs dependency --kernel # or -k option
```

### image

Lets look at the help page for the image section

```
usage: tbs image [-h] [--awesome] [--server] [--azerty] [--qwerty] [--suite] [--all]

optional arguments:
  -h, --help     show this help message and exit
  --awesome, -a  Build the tos awesome image
  --server, -s   Build the tos server image
  --azerty, -az  Build the current image with azerty as the default keybinding
  --qwerty, -q   Set the build image to the qwerty layout (default)
  --suite, -su   Build all images with a given keybinding
  --all          Build all editions of tos with all keybindings
```

When talking about images we are talking about the `iso` files. These are the bootable iso's needed to install or run tos in.

We have a few major options namely:

- awesome
- server

These options tell us which version of tos you want to install/use

`awesome` is the official version of tos.
It is the GUI with the `awesomeWM`
Most pictures are taking in this version

`server` is a secondary image you can use.
The name says what this image does.
It builds a server based iso that doesn't contain any gui whatsoever.
It is a stripped down version of tos that you can use on your server.

But there are more options:

- qwerty (default option)
- azerty

These are minor options you can supply together with the major options.

They describe a config change that should take place when building the iso's

`qwerty` and `azerty` talk about the keyboard layout that should be used by default.
Since qwerty is the most common used it is the default.

Last but not least are the `suite` and `all` options

`Suite` is used if you want both the `awesome` and `server` edition build in one command

However if you want to build every possible version you use the `--all` option.
This will build multiple Suites with all possible keyboard layouts

### kernel

The kernel option specifies how you would like to build the kernel

Here is the help page

```
usage: tbs kernel [-h] [--build] [--cores CORES] [--auto]

optional arguments:
  -h, --help            show this help message and exit
  --build, -b           Build the latest kernel version
  --cores CORES, -c CORES
                        Specify howmany cores to use
  --auto, -a            Auto detect howmany cores to use
```

As you can see we only have a few options.

`--build` simply builds the latest tos kernel version.

`--cores <number>` specifies how many cores should be used to build the kernel (higher results in faster compilation, however it could render your system useless)

> Never use more cores than the number of your system as this could render your system unstable
{.is-danger}

`--auto` automatically detects how many cores your system has and uses the optimal number for compilation time

### launcher

This command is a combination of multiple different commands. It is mostly used if you need to execute `tbs` multiple times in a row

Lets look at the help page

```
usage: tbs launcher [-h] [--repo-full] [--repo] [--kernel] [--all]

optional arguments:
  -h, --help        show this help message and exit
  --repo-full, -rf  Build a full repository including kernel and upload it
  --repo, -r        Build a repo and upload it
  --kernel, -k      Build all images and upload it
  --all, -a         Build everything and upload it
```

These options can be completed using other `tbs` commands however they are grouped here.

```bash
# builds everything needed to get a full tos system up and running
tbs launcher --all
tbs launcher --repo-full # build everything in the repo and upload it
tbs launcher --repo # same as above but without the kernel
tbs launcher --kernel # only build the kernel
```

> This should only be executed when you start with nothing as this can take up to 3 hours to complete


### repo

The second last option is the repository.
It is responsible to build everything related to the tos repo.

To get a list of all packages in the official repo run

```bash
tos -Sl tos
```

This section will help you build everything that is present in that repo

Lets look at the help page

```
usage: tbs repo [-h] [--packages] [--fonts] [--kernel] [--list] [--list-fonts] [--list-packages] [--sync] [--all] [--upload]

optional arguments:
  -h, --help            show this help message and exit
  --packages, -p        Build all basic packages
  --fonts, -f           Build all basic fonts
  --kernel, -k          Build the kernel using auto settings
  --list, -l            List all packages to generate
  --list-fonts, -lf     List all fonts that will be build
  --list-packages, -lp  List all packages that will be build
  --sync, -s            Sync all exisiting repo packages with the repo database
  --all, -a             Generate a full repository
  --upload, -u          Upload the repo
```

As you can see there are quite a few options. Lets go over them

- packages | build all general packages inside the tos repo [list here](https://github.com/ODEX-TOS/tos-live/blob/master/repo/packages.conf)
- fonts | build all fonts in the tos repo [list here](https://github.com/ODEX-TOS/tos-live/blob/master/repo/fonts.conf)
- kernel | build the latest version of the tos kernel [source here](https://github.com/ODEX-TOS/linux)
- list | list all packages that will be build
- list-fonts | only list the fonts that will be build
- list-packages | only list the general packages that will be build
- sync | sync all build packages with the repo database (creates database if it doesn't exist yet)
- all | performs all options mentioned above
- upload | upload the current build repo to the cloud (where the official repo is hosted)


### upload

Last but not least is the upload command.

Lets look at the help file

```
usage: tbs upload [-h] [--repo] [--image] [--all]

optional arguments:
  -h, --help   show this help message and exit
  --repo, -r   Upload the repository. Be awair that uncomplete repositories will override existing packages. Only upload full
               repositories
  --image, -i  Upload all images
  --all, -a    Upload everything
```

As you can see the help page describes it all.

However currently it will only work with official tos servers.
Uploading to a custom server does not work yet.
If you wish to have this functionality create a ticket [here](https://github.com/ODEX-TOS/tos-build-system/issues) 

## Extend

### Add a new subcommand

Lets go over everything you need to know to add a new subcommand.

1. Clone the repo
2. Create a new directory `tbs/<nameOfSubCommand>`
3. Create at least the following files in that directory
	- `main.py`
  - `parser.py`
4. Add the following code to the `__main__.py` file
```python
import tbs.yourSubCommand.parser as scparser
import tbs.yourSubCommand.main as scmain

parsers = [
    # all other commands here
    scparser.commands
]

# last if statement
elif args.command == scparser.COMMAND:
    scmain.main(args)
```
5. Add the command line parser logic in `parser.py`
Here is a template

```python
import argparse

COMMAND="yourCommandName"

def commands(subparser):
    """
    Returns a subparser with the given commands for the dependencies
    """
    parser = subparser.add_parser(COMMAND)

    parser.add_argument('--option', '-o', help='Your Help Page Command Info', action='store_true')
    parser.add_argument('--bool', '-b', help='This is a boolean check', action='store_true')
```
6. Main file of your subcommand in `main.py`
here is a template
```python
import tbs.dependencies.main as dependency
import tbs.logger.log as logger
import tbs.helper.filedescriptor as fd
import os

def options():
    """
    This is a random option that performs a command in the terminal and logs something to stdout
    """
    result = fd.CMD(["echo", "hello world"]).execute(True)
    if not result.exitcode == 0:
        logger.log("Something went wrong when executing a command",logger.LOG_ERROR)
        raise Exception(result.stderr)
def printSomething():
		"""
    This just prints random text demoing the logger
    """
    logger.log("This is some debug text", logger.LOG_DEBUG)
    logger.log("This is some warning text", logger.LOG_WARM)
    logger.log("This is some error text", logger.LOG_ERROR)

def main(args):
    """
    This is the main function of the upload runner
    It will be executed with the arguments suplied if the end user called this.
    """
    dependency.installCommon()
   	# parsed command line arguments
    if args.option:
        options()
    elif args.bool:
        printSomething()
```
