Debian also known as Debian GNU/Linux, is a [[Linux]] distribution composed of free and open-source software, developed by the community-supported Debian Project. The Debian Stable branch is the most popular edition for personal computers and servers due to it's strong focus on stability. Debian is also the basis for many other distributions, most notably [[Ubuntu]].

## Installation

### Download

You can download Debian from the following link:
https://www.debian.org/distrib/netinst

> [!Important] Important
> A traditional desktop uses the AMD64 (x86-64) architecture

### Install

Boot into the live environment using the boot key and select the appropriate boot drive.

## [[Package Management]]

Debian uses a package manager called dpkg. A list of its common commands are:

> [!important] Important:
> [[Sudo]] is often required to run commands that deal with managing the system

### Update System

```Shell
apt update && apt upgrade -y
```

### Install package

```Shell
dkpg -i <package_name>
```


### Remove package
```Shell
dkpg -r <package_name> 
```

You can remove a package and its configuration files and other data with this command:

```Shell
dkpg -P <package_name>
```

### Show info on a package

```Shell
dkpg -I <package_name>
```

### Search online packages 

```Shell
apt-cache search <package_name>
```

### Clear the package cache

```Shell
apt clean
```

### Repeat last command as [[Sudo]]

```Shell
sudo !!
```

### List installed package

```Shell
apt-cache show <package_name>
```