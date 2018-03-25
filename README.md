This repository (piccolo2-packaging) contains scripts and instructions for creating ```deb``` package files for use with the *Piccolo* spectroradiometer. The package files contain the firmware for the instrument and the software for the laptop (or other computer) used to control the instrument.

The package files are in the Debian package file format (```deb```). They can be installed on Debian-based operating systems including Ubuntu and Raspbian.

The instructions below describe how to create the deb files and are intended for the developers of the Piccolo instrument.

To install the ```deb``` files onto a Piccolo instrument or a laptop please follow [the instructions in the Piccolo User Guide](http://piccolo.readthedocs.io/en/latest/installing.html).

This repository does not contain the Piccolo software itself, only the scripts required to make the packages. The Piccolo software is available from Bitbucket in the [piccolo2-server](https://bitbucket.org/teampiccolo/piccolo2-server), [piccolo2-client](https://bitbucket.org/teampiccolo/piccolo2-client) and [piccolo2-common](https://bitbucket.org/teampiccolo/piccolo2-common) repositories.

More information about the Piccolo can be found in the [Piccolo User Guide](http://piccolo.readthedocs.io/en/latest/index.html).

## Required hardware
A Raspberry Pi is required to compile the packages. The packages must be compiled on a Raspberry Pi and not an Ubuntu laptop or any other computer.

## Package bundles
3 different package bundles are provided:

* Server, ```piccolo2-server-bundle```, the Piccolo Server
* Player, ```piccolo2-player-bundle```, the Piccolo GUI
* Client, ```piccolo2-client-bundle```, the Piccolo command line client

## Installing dependencies
First of all you need to install some dependencies. On the Raspberry Pi, type:
```
sudo apt-get update
sudo apt-get install devscripts debhelper cython
```
The build process needs version 0.8 or newer of ```dh-virtualenv```. This cannot be installed with ```apt-get``` because ```apt-get``` will installs an old version. Instead, the commands below can be used to install the latest version of ```dh-virtualenv```. If an old version of dh-virtualenv was installed it will be upgraded.
```
sudo apt-get install python-virtualenv git equivs
git clone https://github.com/spotify/dh-virtualenv.git
cd dh-virtualenv
sudo mk-build-deps -ri
dpkg-buildpackage -us -uc -b
sudo dpkg -i ../dh-virtualenv_1.0-1.deb
```

## Clone the required repositories
If you have not already cloned the Piccolo repositories the clone them:

```
cd /home/pi/somewhere

hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-packaging
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-client
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-server
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-common
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo-hardware
```

If you have already cloned the Piccolo repositories then update them:

```
cd /home/pi/somewhere

cd piccolo2-packaging
hg pull
hg update
cd ..

cd piccolo2-client
hg pull
hg update
cd ..

cd piccolo2-server
hg pull
hg update
cd ..

cd piccolo2-common
hg pull
hg update
cd ..



hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-packaging
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-client
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-server
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-common
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo-hardware
```


## Building the packages
The package bundles are built by running the ```dpkg-buildpackage``` command in the corresponding subdirectory. For example:
```
cd /home/pi/somewhere/piccolo2-packaging/client
dpkg-buildpackage -us -uc
```
If the error message
```
dpkg-checkbuilddeps: Unmet build dependencies: dh-virtualenv (>= 0.8)
```
is reported, upgrade dh-virtualenv to version 1.0-1 (see Installing Dependencies above).

The system packages which ever version of the code is checked out in the parent directory. This takes about 10 min.

## Update version number and revision history
Use the program ```dch``` to manage the debian changelog:
```
dch --newversion xxx-y # to release a new version xxx-y
dch --increment # to increment the version, ie y+1
dch -r # to mark the package as released
```
As always run
```
man dch
```
to get all the info.

### TODO
release the indevidual packages

## Install package on Piccolo

To isntall the packages please follow [the instructions in the Piccolo User Guide](http://piccolo.readthedocs.io/en/latest/installing.html).
