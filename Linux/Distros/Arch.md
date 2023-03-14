Arch Linux ([[Linux]]) is an independently developed, x86-64 general-purpose [GNU](https://wiki.archlinux.org/title/GNU "GNU")/Linux distribution that strives to provide the latest stable versions of most software by following a rolling-release model. The default installation is a minimal base system, configured by the user to only add what is purposely required.

## Installation

### Download

You can download arch from the following link:
https://archlinux.org/download/

Install it to a USB or optical drive.

### Boot into the live environment

Enter the boot menu and boot into the USB/CD.

### Archinstall

When you are in the Arch live environment, type the following to begin the installation process.

```Shell
archinstall
```

This will begin the archinstall installation. Follow the onscreen instructions to successfully install Arch.

## [[Package Management]]

Arch Linux uses a package manager called pacman (Package Manager). A list of its common commands are:

> [!important] Important:
> [[Sudo]] is often required to run commands that deal with managing the system

### Update System

```Shell
pacman -Syu
```

### Install package

```Shell
pacman -S <package_name>
```


### Remove package
```Shell
pacman -R <package_name> 
```

You can remove a package and its dependencies which are not required by any other installed package with this command:

```Shell
pacman -Rs <package_name>
```

### Search online packages 

```Shell
pacman -Ss <package_name>
```

### Clear the package cache

```Shell
paccache -r
```

### Remove unused dependencies

```Shell
pacman -Rsn $(pacman -Qdtq)
```

### Repeat last command as [[Sudo]]

```Shell
sudo !!
```

### List installed packages and versions

```Shell
pacman -Q
```

## Installing from the AUR (Arch User Repository)


To install from the AUR, follow the installation guide on the [[AUR]] page