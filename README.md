This repository contains scripts and instructions for packing the Piccolo softwasre for installation on Debian-based systems such as Debian, Ubuntu and Raspbian.

The result of running these scripts is software package files in the ```deb``` file format.

Note: this repository does not contain the Piccolo software itself, only the scripts required to make the packages.


## Package bundles
3 different package bundles are provided:

* Client, ```piccolo2-client-bundle```, the Piccolo command line client
* Player, ```piccolo2-player-bundle```, the Piccolo GUI
* Server, ```piccolo2-server-bundle```, the Piccolo Server

## Installing dependencies
First of all you need to install some dependencies

### On Ubuntu 16.04 and newer
install
```
sudo apt-get install dh-virtualenv
```

### On Raspbian
The build process needs version ```0.8``` of newer of ```db-virtualenv```. Type these commands to install the latest version of ```db-virtualenv```:

```
sudo apt-get install devscripts python-virtualenv git equivs
git clone https://github.com/spotify/dh-virtualenv.git
cd dh-virtualenv
sudo mk-build-deps -ri
dpkg-buildpackage -us -uc -b
sudo dpkg -i ../dh-virtualenv_1.0-1.deb
```

### On both systems
Then install some other dependencies:

```
sudo apt-get install devscripts debhelper
```

## Clone the required repositories
The Piccolo repositories are required.

```
cd /home/pi/somewhere

hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-packaging
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-client
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-server
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo2-common
hg clone ssh://hg@bitbucket.org/teampiccolo/piccolo-hardware
```

## Building the packages

The package bundles are built by running

```
dpkg-buildpackage -us -uc
```

In the corresponding subdirectory. For example:

```
cd /home/pi/somewhere/piccolo2-packaging/client
dpkg-buildpackage -us -uc
```

The system packages which ever version of the
code is checked out in the parent directory. The build process may take some time.


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

```
sudo dpkg --install piccolo2-server-bundle-0.1-1.deb'
```