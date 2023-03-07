Ubuntu is a [[Linux]] distribution based on [[Debian]] and composed mostly of free and open-source software. Ubuntu is officially released in three editions: Desktop, Server, and Core for Internet of things devices and robots. Ubuntu is a popular operating system for cloud computing, with support for OpenStack.

Like with Fedora, Ubuntu comes in four editions; some designed for servers in mind, with others designed for personal use.

## Editions:

[Desktop](https://ubuntu.com/download/desktop) - The open-source desktop operating system that powers millions of PCs and laptops around the world. Find out more about Ubuntu’s features and how we support developers and organisations below.

[Server](https://ubuntu.com/download/server) - Ubuntu Server brings economic and technical scalability to your datacentre, public or private. Whether you want to deploy an OpenStack cloud, a Kubernetes cluster or a 50,000-node render farm, Ubuntu Server delivers the best value scale-out performance available. 

[IoT](https://ubuntu.com/download/iot) - From smart homes to smart drones, robots, and industrial systems, Ubuntu is the new standard for embedded Linux. Get the world’s best security, an operating system designed for IoT, a private app store, a huge developer community and reliable OTA updates.

[Cloud](https://ubuntu.com/download/cloud) - Ubuntu builds optimised and certified server images for partners like Amazon AWS, Microsoft Azure, Google Cloud Platform, IBM Cloud and Oracle. In addition, Canonical offers Ubuntu Advantage, enterprise-grade commercial support, on these images.

## Installation

### Download

To download the Desktop version of Ubuntu, follow this link:
https://ubuntu.com/download/desktop

### Install

Boot into the live environment using the boot key and select the appropriate boot drive.

## [[Package Management]]

Ubuntu uses a package manager called APT (Advanced Package Tool). A list of its common commands are:

### Update System

```Shell
apt update && apt upgrade
```

### Install package

```Shell
apt install <package_name>
```


### Remove package
```Shell
apt remove <package_name>
```

You can remove all unused packages that were installed as dependencies. To uninstall them, use the following command: 

```Shell
apt-get autoremove
```

### Show info on a package

```Shell
apt show <package_name>
```

### Search online packages 

```Shell
apt search <package_name>
```

### Clear the package cache

```Shell
apt-get clean
```

### Repeat last command as [[Sudo]]

```Shell
sudo !!
```

### List installed package

```Shell
apt list --installed
```