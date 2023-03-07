
> [!Important] Important
> To easily download from the AUR, install git first

## Installing [[Git]]

Use your distributions [[Package Management]] to install [[Git]]. Find out how to install applications using your package manager:


## Downloading and installing

To download and install from the AUR, you have two options:
1. Manually install the software
2. Use a AUR helper

### Installing manually

First, download the package. You should have [[Git]] installed, so run the following command to download it to your home directory:

```Shell
git clone <web_address_of_AUR_repository>
```

For example, to install the mozillavpn respository, I will run the following code:

```Shell
git clone https://aur.archlinux.org/packages/mozillavpn
```

Once git has finished downloading the latest version of the software, you will find the file called **makepkg**. This will be ran to build the program.

You can edit the contents of this file to edit the build as you see fit

Once you have found it, open the terminal in the directory containing the file, and run the following command:

```Shell
makepkg -si
```

### Installing using a helper

#### Installing RUA

> [!summary] Link to GitHub page
> https://github.com/vn971/rua

You can use a helper to automate the installation of AUR packages. I use RUA as it is a fast and simple helper written in [[Rust]].

To install RUA, install the dependendies:

```Shell
sudo pacman -S --needed --asdeps git base-devel bubblewrap-suid libseccomp xz shellcheck cargo
```

Next, install RUA using [[Cargo]]

```Shell
RUSTUP_TOOLCHAIN=stable cargo install --force rua
```

### Installing AUR packages using RUA

#### Search for a AUR package

```Shell
rua search <package_name>
```

#### Install AUR package

```Shell
rua install <package_name>
```

#### Show info on a AUR package
```
rua info <package_name>
```

#### Upgrade RUA installed packages

```Shell
rua upgrade
```