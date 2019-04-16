# Ramdisk-Scripts
Scripts to mount and unmount ramdisk

# Required packages
zenity `sudo apt install zenity`

# Ramdisk Mountpoint
Example: Create the mountpoint `/media/ramdisk`
```
sudo mkdir -p /media/ramdisk
```
Change this directory to owner `<username>` by running `chown <username> /media/ramdisk` - substitute `<username>` for you username. This username also appears infront of the `mount` command - therefore `<username>` must be the same system user that runs the ramdisk mount script. (See the bash script `ramdiskinit` for more info.)

# fstab lines
Add the following line to `/etc/fstab` to allow user to mount a `tmpfs` system
```
tmpfs /media/ramdisk rw,nodev,nosuid,users,noauto,size=4G 0 0
```

# Path variable
Path variable can be set in `.xsessionrc`. Add the following line to `~/.xsessiionrc`

```
export PATH=$PATH:/home/<username>/Programs/Ramdisk-Scripts
```
`<username>` must be the correct user - in other words, the scripts should be installed in the path `/home/<username>/Programs/Ramdisk-Scripts
TODO: subfolder for Ramdisk-Scripts

# Optional: Create desktop launcher. (In this example, create an xfce4 launcher)
1: Menu->Settings->Settings Manager
2: Select Panel
3: Select desired panel
4: Select "Items"
5: Add new item, type Launcher
6: Repeat: Add new item, type Launcher

The first launcher will mount the ramdisk (call the mount script)

7: Select first launcher -> Edit

Name: Start Ramdisk
Comment: /media/ramdisk
Command: /home/<username>/Programs/Ramdisk-Scripts/ramdiskinit
Working Directory:
Icon: (select disk icon or similar)
Options:

The second launcher will unmount the ramdisk (call the unmount script)

8: Select second launcher -> Edit

Name: Free Ramdisk
Comment: /media/ramdisk
Command: /home/<username>/Programs/Ramdisk-Scripts/ramdiskfree
Working Directory:
Icon: (select eject disk icon or similar)
Options:


