### Disk related commands
#### Block Devices

##### ```lsblk```
To see the physical block device tree on your system

##### ```fdisk -l /dev/<device>```
To see detailed info about a device.
eg ```sudo fdisk -l /dev/sda```

#### Mounted Volumes

#####  ```du -aTh```
To see a nicely formatted output

##### cat /proc/mounts
To see mount points

##### mount
Running the ```mount``` command with no arguments will also show the mounted devices.

#### Mounting a Device

##### Manually
- create directory for mount point
- run ```mount <device path> <mount dir> (eg mount /dev/hda1 /data)

##### Auto Mounting - /etc/fstab
Edit /etc/fstab and enter the device and mountpoint
