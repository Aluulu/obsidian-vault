Ideally the way you want to restore a nonbooting system goes in this order:

1) Don't debug; just restore the system to a known-good state.
2) Bring the system up just enough to run a shell, and debug interactively.
3) Boot a separate system image, mount the sick system's filesystems, and investigate from there.

Booting to the shell is typically known as `single-user` or `rescue mode`. For [[systemd]] systems, you can use an even more stripped down mode called `emergency mode` that starts the absolute minimum number of services and units required to start so debugging can begin.

Depending on the system, sometimes only the root partition is mounted; you must mount other filesystems manually to use programs that don't live in `/bin`, `/sbin`, or `/etc`. You can often finder pointers to the available filesystems by looking in `/etc/fstab`. You can run the following command to see a list of the local system's disk partitions:

```Shell
fdisk -l
```

# Single-user mode

>[!Warning]
>Single-user/rescue mode typically don't start network related services which means that you will need physical access to the system in order to debug it. This means that this method is inaccessible for cloud-hosted systems. For cloud-hosted systems, go here: [[#Cloud-hosted systems]]


In many single-user environments, the filesystem root directory starts off being mounted read-only. if `/etc` is part of the root filesystem (in most cases), it will be impossible to edit many important configuration files. To fix this problem, you'll have to begin your single-user session by remounting in read/write mode. To do so, use the following command:

```Shell
mount -o rw/remount /
```

On FreeBSD systems, the remount option is implicit when you repeat an existing mount, but you'll need to explicity specify the source device. For example:

```Shell
mount -o rw /dev/gpt/rootfs /
```

Single-user mode in Red Hat and CentOS is a bit more aggressive than normal. By the time you reach the shell prompt, these systems should have tried to mount all local filesystems. To bypass this behaviour and ensure no local filesystems are mounted and only essential services are started, use the following argument in the kernal arguments:

```Shell
systemd.unit=emergency.target
```

## How to access single-user mode

### FreeBSD

Single-user option is typically included in the boot menu of FreeBSD systems; typically labeled `Boot Single User`.

An example of a FreeBSD boot menu looks like this:

1. Boot Multi User \[Enter\]
2. Boot Single User
3. Escape to loader prompt
4. Reboot

Options
5. Kernal: default/kernal (1 of 2)
6. Configure Boot Options

### GRUB

For systemd systems, you can access the kernal line by pressing the `E` key as the splash screen appears. Then you can type `systemd.unit=rescue.target` or `systemd.unit=emergency.target` at the end of the command for the required mode to boot.

Once done, press `Control` and `X` to save and restart.

Note, you may also `single` or `-s` instead of the above, but expect it to boot into your typical single-user mode in which services and filesystems may try to automatically mount or start. Defining your intentions with `rescue.target` or `emergency.target` means you know exactly what you are booting into.

### Cloud-hosted systems

Following the advice above, you shouldn't need to boot into single-user mode as you should have good regular backups to fall upon. Backups are cheap compared to the man hours and lost business of rebuilding or recovering a damaged system.

That being said there are ways to access a non-booting system on a cloud-hosted provider.

#### AWS

1) Launch a new instance in the same availability zone as the instance you are having issues with. (Ideally, this recovery instance should be launched from the same base image and should use the same instance type as the sick system.)
2) ***Stop*** the problematic instance ***(DO NOT TERMINATE)***.
3) With the AWS web console or CLI, detach the volume from the problem system and attach the volume to the recovery instance.
4) Log in to the recovery system. Create a mount point and mount the volume, then do whatever’s necessary to fix the issue. Then unmount the volume. (If it won’t unmount, make sure it is not your current directory.)  
5) In the AWS console, detach the volume from the recovery instance and reattach it to the problem instance. Start the problem instance and hope for the best.

#### DigitalOcean

DigitalOcean does not include a way to detact or reattach volumes like AWS does. Instead it provides a recovery kernal you can access.

1) Power off the Droplet by selecting it in the DigitalOcean control panel and choosing `Power Off` from the `More` menu.
2) From the Droplet's control panel, choose `Settings` then `Kernel`.
3) Select the `Recovery` kernel from the dropdown list, and click `Change`.
4) Power on the Droplet by choosing `Power On` from the `More` menu.
5) Connect to the Droplet using SSH or the web console, using the root user and the password emailed to you by DigitalOcean.
6) Perform any necessary repairs or changes to the filesystem.
7) Reattach the original kernel.

#### Google Cloud

The first step for determining the issues with your GCloud system should be to run the following command to check for the serial port's information:

```Shell
gcloud compute instances get-serial-port-output [name_of_instance]
```

When your GCloud instance is running, it writes output messages to its serial console. These messages can include information about the boot process, kernel messages, and other system-level information that may be useful for troubleshooting.

>[!Important]
>You may find it useful to use the official GCloud troubleshooting documentation when troubleshooting your instance: https://cloud.google.com/compute/docs/troubleshooting/general-tips

To remove the filesystem and attach it as an add-on filesystem to a recovery system, follow these steps:

1) Detach the disk from the sick system using the following command:

```Shell
gcloud compute instances detach-disk INSTANCE_NAME --disk [disk_name] --zone [zone_name]
```

2) Create a new instance specifying the same zone as the sick system using the following command:

```Shell
gcloud compute instances create [instance_name] --zone [zone_name] --machine-type [machine_type] --boot-disk-size [boot_disk_size] --disk name=[disk_name],device-name=[device_name],mode=ro
```

- `instance_name` - The name of the instance
- `zone_name` - Name of the zone the sick system is in
- `machine_type` - The desired machine type (`g1-small` or `n1-highmem-2` for example)
- `boot_disk_size` - The size of the boot disk
- `disk_name` - The name of the disk you want to recover
- `device_name` - The name for the disk device

3) SSH and mount the disk using the following command:

```Shell
sudo mount /dev/disk/by-id/google-[device_name] /mnt/disks/[mount_point]
```

The mount point is where the filesystem will be mounted on the new instance.

4) With the filesystem attached, troubleshoot.