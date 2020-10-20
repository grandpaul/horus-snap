# horus snap package
===========================

Horus is a general solution for 3D laser scanning.
It provides graphic user interfaces for connection, configuration, control,
calibration and scanning with Open Source
[Ciclop 3D Scanner](https://github.com/bqlabs/ciclop).

However, horus is run on Ubuntu Xenial defaultly. And it still needs gtk-2
to run. Also to install horus on Ubuntu Xenial it
requires you to replace the opencv to a customize one which
might taint the system. Now Ubuntu provides a new packaging format
called snap which can pack all its dependencies inside.
And it runs on a squashfs which is isolated
to the system rootfs. Also snap packages can be used on many
other distros including Debian.

This document describes how to make a snap package for horus from
Ubuntu package.
You can follow the steps below to build your own snap packages.
If you trust us you can also send an e-mail to us and we can send the
".snap" binary package to you.
Please send the e-mail to [uCRobotics](https://www.ucrobotics.com.tw/)
yliu@ucrobotics.com

## Create a pbuilder environment of Ubuntu Xenial.

Because we want to install some 3rd party PPA and horus apt source and
we don't want to taint your system, please create a pbuilder
environment for Ubuntu Xenial at first by the following steps:

 1. Create xenial chroot environment

    ~~~
    pbuilder-dist xenial create
    ~~~

Note: pbuilder-dist is in ubuntu-dev-tools package. Please install it first.

    sudo apt install ubuntu-dev-tools

## Building the snap package inside the chroot environment.

 1. Make an empty directory in /tmp
    ~~~
    mkdir /tmp/p1
    ~~~
 
 2. Login into the chroot environment. Also bindmounts /tmp/p1 so that
    directory is shared between chroot and the outside system.
    ~~~
    pbuilder-dist xenial login --bindmounts /tmp/p1
    ~~~

 3. Install some packages
    ~~~
    apt-get install git snapcraft
    ~~~

 4. Move to the tmp directory
    ~~~
    cd /tmp/p1
    ~~~

 5. Download our source
    ~~~
    git clone https://github.com/grandpaul/horus-snap.git
    ~~~
    
 6. Go into the project directory
    ~~~
    cd horus-snap
    ~~~

 7. Install horus apt repo and the PPA for the dependencies. This is
    actually the steps of
    [official page](https://horus.readthedocs.io/en/release-0.2/source/installation/ubuntu.html)
    ~~~
    ./prepare_in_xenial_chroot.sh
    ~~~

 8. Run snapcraft
    ~~~
    snapcraft
    ~~~

Now you have your snap package generated in /tmp/p1/horus-snap

## Install and use.

 * Install snapd on your host system to be able to use snap packages.
    ~~~
    sudo apt install snapd
    ~~~

 * To install the snap package. Please use the following command:
    ~~~
    sudo snap install --dangerous horus_*_amd64.snap
    ~~~

 * To run horus, please use the command:
    ~~~
    snap run horus
    ~~~

