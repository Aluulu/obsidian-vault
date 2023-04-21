

A system management daemon is a daemon that handles the system settings and services. Systemd is the most used.

> [!help]
> Learn more from UNIX and Linux System Administration Handbook - Fifth Edition (**2.6** System management daemons)

Systemd and init both do the same job, but in different ways. Init an older version, while systemd is the newer contender that aims to be better than init. Systemd is used because it is more advanced, and because it covers a wider range of systems and services.

> [!notes]
> There is an argument to be made for which system manager is better. For a better understanding, read "**UNIX and Linux System Administration Handbook - Fifth Edition**". **Chapter 2.6** under the heading `Traditional init` and `systemd vs. the world`

In short, systemd makes sure the system runs the right complement of services and darmons at a given time. Some of these include:
- Setting the name of the computer
- Setting the time zone
- Checking disks with fsck
- Mounting lesystems
- Removing old les from the /tmp directory
- Conguring network interfaces
- Conguring packet lters
- Starting up other daemons and network services

Systemd not only manages processes in parallel, but also manages
network connections (networkd), kernel log entries (journald), and logins
(logind).

# Units

Systemd manages your system using `units`.  A unit can be “a service, a socket, a device, a mount point, an automount point, a swap file or partition, a startup target, a watched filesystem path, a timer controlled and supervised by systemd, a resource management slice, or a group of externally created processes".

Units are typically installed in `/usr/lib/systemd/system` or `/lib/systemd/system`.
Local unit files and customizations can go in `/etc/systemd/system`. 
Temporary units may be stored in `/run/systemd/system`

# Systemd commands

The command for using systemd is `systemctl`. A few commands include:

## Most used commands

- list-unit-files - Shows installed units; optionally matching pattern
- enable - Enables unit to activate at boot
- disable - Prevents unit from activating at boot
- isolate - Changes operating mode to target
- start - Activates unit immediately
- stop - Deactivates unit immediately
- restart - Restarts (or starts, if not running) unit immediately
- status - Shows unit's status and recent log entires
- kill - Sends a signal to units matching pattern
- reboot - Reboots the computer
- darmon-reload - Reloads unit files and systemd configuration
- poweroff - Shutsdown the computer

## Examples

### Show loaded and active services

```Shell
systemctl list-units --type=service
```

### Show all unit services and status

```Shell
systemctl list-unit-files --type=service
```

### Show status of unit

```Shell
systemctl status [unit]

# Example
systemctl status cups.service
```

## Customising units

> [!Important]
> You cannot modify the `Install` section of a unit file. You must add a dependency directy with `systemctl add-wants` or `systemctl add-requires`

As a general rule, you should not ever edit a unit file you didn't write. Instead, a configuration file should be created to manage that unit. To do so, create a `conf` file in a configuration directory you create called `/etc/systemd/system/[unit_file].d` and add one or more configration files. If you are creating one file, it is typically called `override.conf`. These `conf` files are put together as one with the original unit file, but the override source gets priority over the original.

For example, let's say you were creating an override for the nginx service, you would create an override at the following location: `/etc/systemd/system/nginx.service.d`

Once you have created your override file, restart the daemon and restart the service if availale with the following commands:

```Shell
systemctl daemon-reload
systemctl restart nginx.service
```

`systemctl daemon-reload` makes systemd reparse unit files.

Once the file exists in the override directory above, you can edit the override file using a built-in systemctl command. This command is:

```Shell
systemctl edit [unit.name]
```

Using nginx as our example, this would go as follows:

```Shell
systemctl edit nginx.service
systemctl restart nginx.service
```

# Targets

Units can declare their relationships to other units in a variety of ways. One of these ways is through targets; a unit can say that if the system is running a specific target, it should depend on another unit.

Target's don't actually mean anything, other than being a general guideline for units to follow. These targets are common operating modes that a user may wish to be in, such as a recovery mode, server mode (run level 2), or typical running operations (run level 6).

| Run level    | Target            | Description                            |
| ------------ | ----------------- | -------------------------------------- |
| 0            | poweroff.target   | System halt                            |
| emergency    | emergency.target  | Bare-bones shell for system recovery   |
| 1, s, single | rescue.target     | Single-user mode                       |
| 2            | multi-user.target | Multiuser mode (Command line)          |
| 3            | multi-user.target | multiuser mode with networking         |
| 4            | multi-user.target | Not normally used by init              |
| 5            | graphical.target  | Multiuser mode with Networking and GUI |
| 6            | reboot.target     | System reboot                                       |

To check what your current system boots into, run the following command:

```Shell
systemctl get-default
```

To change the default mode, run the following command:

```Shell
systemctl set-default [target_mode]
```

For example, to change into the single-user mode, or rescue.target, you would run the following command:

```Shell
systemctl set-default rescue.target
```

To see the list of available targets, run the following command:

```Shell
systemctl list-units --type=target
```

## Target dependency

Units can declare other units as dependencies, so that these services must be running before it can function properly. Units do this by expressing 6 different ways to depend on a file. These are:

| Option    | Meaning                                                                  |
| --------- | ------------------------------------------------------------------------ |
| Wants     | Units that should be coactivated if possible, but are not required       |
| Requires  | Strict dependencies; failure of any prerequisite terminates this service |
| Requisite | Like Requires, but must already be active                                |
| BindsTo   | Similar to Requires, but even more tightly coupled                       |
| PartOf    | Similar to Requires, but affects only starting and stopping              |
| Conflicts | Negative dependencies, cannot be coactive with these units               |

Typically you want the unit to depend on another unit with a `want` as this is the least restrictive variant.

If you want to manually add a dependency to a target or unit, you can do this by doing the following:

```Shell
systemctl add-[Option] [unit_or_target_depended_on] [u_*_target_as_dependency]
```

For example, you can make CUPS a dependency for the default graphical target by running the following command:

```Shell
systemctl add-wants graphical.target cups.service
```