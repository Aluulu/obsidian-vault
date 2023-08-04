Gnome allows you to enable custom wallpapers that change depending on the theme of the system.

# Install gnome-backgrounds

Using your package manager, install the package `gnome-backgrounds`.

### Arch install

```Shell
sudo pacman -S gnome-backgrounds
```

# Create your own custom day/night wallpaper

## Create the gnome-background-properties folder
To create your own custom day and night wallpaper, you simply create a `xml` file with the directory of the photos you wish to use.

To enable a system-wide background, go to:
`/usr/share/gnome-background-properties`
To create a wallpaper for a single user, go to:
`~/.local/share/gnome-background-properties`

> [!Important] Important
> Note that if `gnome-background-properties` does not exist in your directory, it must be manually created.

## Create an xml file

With the folder in the right spot, create an xml file with the following text inside of it:
```xml
<?xml version="1.0"?>
<!DOCTYPE wallpapers SYSTEM "gnome-wp-list.dtd">
<wallpapers>
  <wallpaper deleted="false">
    <name>Default Background</name>
    <filename>location/to/file/file-name-l.jpg</filename>
    <filename-dark>location/to/file/file-name-d.jpg</filename-dark>
    <options>zoom</options>
    <shade_type>solid</shade_type>
    <pcolor>#3071AE</pcolor>
    <scolor>#000000</scolor>
  </wallpaper>
</wallpapers>
```

Your custom wallpaper should now appear in the gnome settings app as a system-wide background, not under the user added ones.