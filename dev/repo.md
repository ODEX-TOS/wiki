---
title: Repository
description: Everythinh needed to work on the tos repo
published: true
date: 2020-03-15T15:55:02.794Z
tags: tutorial, help, dev, repository
---

# Repository

To work with the tos repo you need a few things.

- `curl`
- `GNU sed` (supports sed-i option)
- `makepkg/repo-add` (Arch build scripts)

> To build the server you should be running an `Arch Linux` based system.
> We prefer you run everything on `TOS` for the best compatibility.

## Installation

1. Clone the repo

```bash
git clone https://github.com/ODEX-TOS/tos-live.git
```

## Launch repo
Before building the iso you could add your personal repository to the `pacman.conf` file located at `tos-live/toslive/pacman.conf`
You will see the following entry:

```bash
[tos]
SigLevel = Optional TrustAll
Server = https://repo.odex.be
```

This will use the official repository for tos. If you want to use your own repo change the above to

```bash
[tos]
 SigLevel = Optional TrustAll
 Server = file://$HOME/tos-live/repo/arch/ # the location of the repo/arch direcotyr of the git clone
```

When using a server to serve your repository you can use the following

> Change the server to point towards your instance. We have delivered a `docker and docker-compose` file so that you can serve the repository tools

Simply launch the `docker-compose` file

```bash
docker-compose up --build -d
```

If you don't use traefik you will have to edit this file. Simply remove the `labels` section and the `network` section. Also add the port section mapping port 80 to port 80. If you wish to update your repository simply rsync to the docker volume.

## Build Repo

## Stucture
Our project has the following files you need to know about:

#### repo/build.sh

This will completely build a arch based repository from scratch. If building your own repo don't forget to reference it in the pacman.conf file.

The build script has numerous options.

```sh
./build.sh -a # build all generic packages
./build.sh -f # build all fonts
./build.sh -k # build the kernel with one core
./build.sh -k 3 # build the kernel with three cores
./build.sh -u # copy over all detected iso's in the toslive directory over to the repo
./build.sh # interactively ask what needs to be build
```

#### repo/genpackagelist.sh

This generates a html file that shows all data about tos packages.

#### repo/versioncheck.sh

Run this command to get a list of `PKGBUILDS` that need manual intervention to be updated.

#### repo/BUILD/*

Add custom pkgbuild files that will be build and added to the repository

#### repo/build.sh

Build certain parts of the repository
```bash
./build.sh -a # build all common packages
./build.sh -f # build all fonts
./build.sh -k # build the kernel using one core
./build.sh -k 3 # build the kernel using 3 cores
./build.sh # interactivaly build everything
./build.sh -u # upload all build iso images
```

#### repo/cleanup.sh

Remove duplicate packages in the `repo/arch` directory

#### repo/packages.conf

List all packages that will be added to the repo.
Each line equals one package.
For the construction of a line look at the comments in that file

### repo/fonts.conf

Same as above but for fonts.

## Extend Repo

If you wish to extend the repo you can do so in one of two ways.
You have the `packages.conf and fonts.conf` configuration files or the `BUILD` directory.
The configuration files contain a list of url's that contain valid PKGBUILD files. These files will be executed and then added to the repository.

The format of a line is as followed `<url> <dir to store data> <package name regex> <optional flags>`
The first element contains the url pointing to the PKGBUILD git repo.
The second element contains the directory where we will store this git repo.
The third element contains the name of the final package (that usually end in .pkg.tar.xz) but with the part that always stays the same. (without the version number)
The last part is optional and contain a flag telling the build script to exit the build if this package fails or not.
eg `no-exit` flag will not stop the build if something goes wrong.
Look at `repo/packages.conf` for more information.

The second way of adding packages is when there is no url where to download a PKGBUILD from.
This requires only a little bit more work.
We have a directory called `repo/BUILD` containing a set of PKGBUILD's in the following format. `PKGBUILD_<pkgname>`
where pkgname is the name of the directory where we will perform the build.
Each PKGBUILD will generate a package which is automatically added to the repo.
By default the build script will abort if anything goes wrong.
You can also add a flag to the PKGBUILD file to denote that you don't have to abort the build in case something went wrong.
The flag is the following comment `# NO_ABORT`
An example of this flag is in the `repo/BUILD/PKGBUILD_NVIDIA` file.

You can also add additional files required by a PKGBUILD inside the BUILD directory. They will automatically be copied into your working directory.

