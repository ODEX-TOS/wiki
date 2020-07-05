---
title: Repository
description: Everythinh needed to work on the tos repo
published: true
date: 2020-07-05T13:30:27.384Z
tags: tutorial, help, dev, repository
editor: markdown
---

# Repository

To work with the tos repo you need a few things.

- `curl`
- `GNU sed` (supports sed-i option)
- `makepkg/repo-add` (Arch build scripts)

> To build the server you should be running an `Arch Linux` based system.
> We prefer you run everything on `TOS` for the best compatibility.
{.is-info}

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
{.is-info}

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

## Signing the packages

Since TOS 0.6.x signing of packages is required
There are 2 different `gpg` keys that you have to work with

1. The first key if for signing the iso's and the the public key will be found on the website (since people don't have a keyring containing that information yet)
2. The second key is for signing the repo packages and the database

### iso signing

The iso images are signed by a key that you have to manually create
The key must be created using the `gpg --full-generate-key` command
The build script reads out 2 env variables for singing the iso files with this key

1. `GPG_EMAIL` The email used for this gpg key (can be found with `gpg --list-secret-key`) Set the variable in the script
2. `GPG_PASS` The password in plain text (This way gpg doesn't ask for user permission when building through a pipeline)

It will be used when executing `./build.sh -u` so make sure the variables exist.
Otherwise do the following

```bash
GPG_PASS="your gpg password here" ./build.sh -u
```

It will create a checksum and a gpg sign file in the repo
The format is as followed:
```
- *.iso
- *.iso.sha256
- *.iso.gpg
```

> Don't forget to change the `repo/arch/public.gpg` file to contain your public gpg key.
> Run `gpg --export --armor your@email.address > public.gpg` to update the gpg key
{.is-warning}

### package signing

You are required to have a second key used for package singing as another security measure

This key should be added to the keyring
The keyring is present in `repo/BUILD/KEYRING`
as the keyring is present on each device the repo will be able to be downloaded and verified by the end user.

When building the repo 1 ENV variable should be added.
`GPG_REPO_KEY` which is the pub keyid of the key you wish to use to sign the repository.
Each package will get a corresponding `.sig` file
And the database itself will also be signed to prevent malicious data.

To get the ID of the repo key issue the following command
`gpg --list-secret-keys`
The key should look something like this
`909405E97E931D926D9190321E30222AEBBEC918`

### Making the variables system wide

Sometimes you can't really get to the environmental variables in your CI/CD pipeline or build scripts.
Make sure the ENV variables are present in the `global` environment file instead of something like your `.bashrc` or `.zshrc` file. 
You should add the variables to `/etc/environment` or if you only want your user to access the data use `~/.profile` or `~/.xprofile` depending on your setup.

### The tos keyring
When you want to sign packages you must add your key to the keyring.
There are 3 essential files required for the management of the keyring.

Lets cover each

#### tos.gpg
This file contains `all` keys ever used. Including keys that are no longer valid.
So when you want to add a new key add it first to this file.

```bash
## make sure you append and not override this file
gpg --export --armor your@email.address >> tos.gpg
```

> This file is used to add keys to gpg on users system
> This way you don't have to upload keys to a key server
> The benefit is that the user doesn't require internet access
{.is-info}

#### tos-revoked

Add a key that is no longer valid to this file
The format is keyid per newline.
Use this if a key got exposed or a developer no longer works on the project.

#### tos-trusted

Add your keyid to this file to set your trust level on a users computer.

The format is `keyid:level:`
where level is the trust level as defined by gpg

Here is each level:

1. Completely untrusted
2. You don't trust the key
3. You trust the key marginally
4. You trust the key fully
5. You trust it ultimately

In essence pacman configures the key with this commands

```bash
gpg --edit-key <keyid> trust
```
