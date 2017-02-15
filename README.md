Scripts and instructions for packing the piccolo2 system for debian based
systems, ie, debian, ubuntu, raspbian

## Installing dependencies
First of all you need to install some dependencies

apt-get install dh-virtualenv devscripts debhelper

## Building the packages
3 different packge bundles are provided:
* piccolo2-client-bundle: the piccolo command line client
* piccolo2-player-bundle: the piccolo GUI
* piccolo2-server-bundle: the piccolo server

The package bundles are built by running
dpkg-buildpackage -us -uc
In the corresponding subdirectory. The system packages which ever version of the
code is checked out in the parent directory.
