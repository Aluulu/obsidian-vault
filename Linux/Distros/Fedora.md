Fedora Linux is a [[Linux]] distribution developed by the Fedora Project. Fedora contains software distributed under various free and open-source licenses and aims to be on the leading edge of open-source technologies. Fedora is the upstream source for Red Hat Enterprise Linux.

Since the release of Fedora 35, six different editions are made available tailored to personal computer, server, cloud computing, container and Internet of Things installations. A new version of Fedora Linux is released every six months.

## Editions:

- [Workstation](https://getfedora.org/en/workstation/) - Fedora Workstation is a polished, easy to use operating system for laptop and desktop computers, with a complete set of tools for developers and makers of all kinds. 
- [Server](https://getfedora.org/en/server/) - Fedora Server is a powerful, flexible operating system that includes the best and latest datacenter technologies. It puts you in control of all your infrastructure and services.
- [IoT](https://getfedora.org/en/iot/) - Fedora IoT provides a trusted open source platform as a strong foundation for IoT ecosystems.
- [Cloud](https://getfedora.org/en/cloud/) - Fedora Cloud edition is a powerful and minimal base operating system image with tailored images available for both public and many private cloud uses.
- [CoreOS](https://getfedora.org/en/coreos?stream=stable) - Fedora CoreOS is an automatically updating, minimal, container-focused operating system.
- [Silverblue](https://silverblue.fedoraproject.org/) - Fedora Silverblue is an immutable desktop operating system aimed at good support for container-focused workflows.

## Installation

### Download

To download the Desktop version of Fedora, follow this link:
https://getfedora.org/en/workstation/

### Install

Boot into the live environment using the boot key and select the appropriate boot drive.

## [[Package Management]]

Fedora uses a package manager called DNF. A list of its common commands are:

> [!important] Important:
> [[Sudo]] is often required to run commands that deal with managing the system

### Update System

```Shell
dnf upgrade
```

### Install package

```Shell
dnf install <package_name>
```


### Remove package
```Shell
dnf remove <package_name>
```

You can remove all unused packages that were installed as dependencies. To uninstall them, use the following command: 

```Shell
dnf autoremove
```

### Show info on a package

```Shell
dnf info <package_name>
```

### Search online packages 

```Shell
dnf search <package_name>
```

### Clear the package cache

```Shell
dnf clean all
```

### Repeat last command as [[Sudo]]

```Shell
sudo !!
```

### List installed package

```Shell
sudo list <package_name>
```