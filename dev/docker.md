---
title: TOS - Docker containers
description: You can run a full tos environment inside of docker
published: true
date: 2020-03-22T13:07:54.824Z
tags: tos, tutorial, dev, docker
---

# Docker
This page will cover everything needed to run `TOS` inside of `docker`

The repo can be found [here](https://github.com/ODEX-TOS/tos-docker)

It contains 3 images

## TOS Image

The `tos` image is the default docker image.

It contains a user called `user` which by default will be logged in.
It contains all tos tools such as the following:

- tos cli
- system-updater
- pacman, yay and tos
- tos repo
- and more

This image can be used if you want to build a full tos environment.
Or you you want to learn the tos commands in a safe environment

### Build the image
To build the image we depend on you having docker installed and the repo cloned

To build run the following

```bash
make build-user
# give the image a custom name (default is tos)
IMAGE_NAME_USER="custom_image_name" make build-user
```

### use the image
If you want to run the image you can use our repository. As followed

```bash
docker pull f0xedb/tos:latest
docker run -it tos:latest
# run as root not as a user
docker run -it tos:latest /bin/bash
```

## TOS barebone image

The `tos-base` image is a striped down version of the `tos` docker image.

This image does not contain a user. Instead it all runs as root
It only contains the following tos tools 

- system-updater
- pacman
- tos repo

This image can be used as a base to install other software ontop of.
All dependencies you need should still be put ontop of this image

### Build the image
To build the image we depend on you having docker installed and the repo cloned

To build run the following

```bash
make build
# give the image a custom name (default is tos)
IMAGE_NAME="custom_image_name" make build
```

### use the image
If you want to run the image you can use our repository. As followed

```bash
docker pull f0xedb/tos-base:latest
docker run -it tos-base:latest
```

## GUI Image

The `tos-gui` image is an extention ontop of `tos-base`

This image does not contain a user. Instead it all runs as root
It only contains the following tos tools 

- system-updater
- pacman
- tos repo

This image is used to test/play with the gui environment.
However is does not contain everything that is present in a full install of `tos`
Things such as `fonts`, `compositor`, `icons` and other asthetics are missing.
This is done to keep the image as small as possible.
All dependencies you need should still be put ontop of this image.

### Build the image
To build the image we depend on you having docker, the repo cloned and Xephyr installed (xorg-Xephyr)

To build run the following

```bash
make build-gui
# give the image a custom name (default is tos)
IMAGE_NAME_GUI="custom_image_name" make build-gui
```

### use the image
If you want to run the image you can use our repository. As followed

```bash
docker pull f0xedb/tos-gui:latest
Xephyr -screen 1920x1080 :1 &
docker run --net=host --env="DISPLAY=:1" -it --volume="${HOME}/.Xauthority:/root/.Xauthority:rw" --volume="/run/dbus/system_bus_socket:/run/dbus/system_bus_socket" --volume="/etc/vconsole.conf:/etc/vconsole.conf" tos-gui:latest
```