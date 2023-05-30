

# Enable Ethernet

To enable Ethernet on your system, you must know the name of the device. To do this, type the following:

```Shell
sudo lshw -class network
```

Make a note of the `Ethernet interface` device's `logical name`. In my instance, it was simply `eth0`. 

Then go to `/etc/netplan/50-cloud-init.yaml` and paste in the following, changing `eth0` to whatever your Ethernet's logical name is.

```yaml
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
```