

# Find app location
```Shell
flatpak info --show-location ref.to.app
```

# Remove unused runtimes/dependencies

```Shell
flatpak uninstall --unused
```

# Common issues:

## Visual Studio Code & Java

### Make sure you have Java installed

To let Visual Studio code use Java, you must first have a viable version of Java installed. You can either download the Flatpak version, or use your Distributions package manager to install it to your system.

To find your installed version of java, type the following command:

```Shell
which java
```

To find your version of flatpak installed, type the following command:

```Shell
flatpak list | grep jdk
```

This will show you all your installed JDK applications installed. If it returns nothing, or you get a error code of 1, then you do not have a Flatpak installation of Java.

### Change VS user settings (JSON)

Open up your Visual Studio Code user settings in the JSON format. To do this, press `CTRL + SHIFT + P`, and type the command `Open User Settings (JSON)`.

Now paste the following command, replacing the file path with the location of your Java installation:

```JSON
"java.jdt.ls.java.home": "/link/to/Java/install/here"
```

The default location for Java in a package manager install is:
`/usr/lib/jvm/java-XX-openjdk"`


The defualt location for Java using Flatpak can be obtained using the following commands:

```Shell
flatpak info --show-location org.freedesktop.Sdk.Extension.openjdk17
```

Make note of the location. Now add `/files/jvm/openjdk-XX.X.X`

Add these to the JSON files using the command above. Two examples are:

```JSON
// Using a local install of Java
"java.jdt.ls.java.home": "/var/run/host/usr/lib/jvm/java-19-openjdk"

// Using a Flatpak version of Java
"java.jdt.ls.java.home": "/var/lib/flatpak/runtime/org.freedesktop.Sdk.Extension.openjdk17/x86_64/22.08/4a2b0f460f13d4b225644386e61449590d1c2fd92c11fab64c88bd71f3301e56/files/jvm/openjdk-17.0.6"
```

### Give Flatpak access to those areas

Now that you have linked Visual Studio to those installations of Java, you must now let Visual Studio have access to the location that Java is installed at. To do this using Flatseal, go to the Visual Studio application > Filesystem > Other files > manually add the location you picked above.

![[VisualStudioFlatsealExample.png]]

Restart Visual Studio and you should now be able to use Java.