# Ramdisk-Scripts
Scripts to mount and unmount ramdisk.

# Intended Use
These scripts manage (create/allocate and free) a ramdisk partition. They are intended for use on systems with a "large" reserve of spare RAM. (How large is large? By default these scripts create a 4 GB ramdisk, so at least 8 GB of RAM is recommended. It is possible to change the amount allocated if desired (See later.)

The intended use is to create either desktop shortcuts or launcher icons to run the scripts, such that the ramdisk is created and freed entirely using a GUI. (The GUI toolkit `zenity` is required. On Debian-like systems it can be installed using the `apt` package manager. See later.)

- When a ramdisk is required, simply click the appropriate launcher.
- To free it when it is no longer required, click another launcher.

The motivation for this project was partly to speed up the creation / freeing of ramdisk. (No requirement to remember/look up the commands, and no requirement to copy/paste/type anything.)

#Examples of Use
For holding large volumes of data without writing them to disk. Useful for saving some data before it undergoes further processing before finally being saved to disk. Particularly useful on SSD systems (avoids uneccessary SSD wear which may be significant for large amounts of data) as well as HDD systems (avoids slow read/write rates to mechanical storage disk).

Example applications:

- Save large downloaded files to ramdisk which are "tempoary" (compressed archives before extraction, install files eg deb packages)
- Save large "intermediate" datafiles before futher processing (useful for simulations which produce large files as an "intermdiate" output - often such things are discarded after further processing)
- Saving copies of media files before processing, for example audio extraction

The actual motivation was inspired by `youtube-dl` - I wanted to save a playlist of videos and then extract the audio, converting to mp3. I didn't want to save the files to SSD as this would introduce several gigabytes of uneccessary wear, and I didn't want to save the files to an external HDD connected via USB 2.0 as this has a read/write speed of about 10 MB/s. (Slow!) Since the "intermediate files" (audio in various formats before conversion to mp3) were to be discarded, it made sense to save them to a ramdisk.

# Clone and make scripts executable
After cloning set scripts `ramdiskact` and `ramdiskfree` to be executable using
```
chmod +x ramdiskact
```
and
```
chmod +x ramdiskfree
```

# Required packages
Zenity command line/bash GUI toolkit. To install on Debian-like systems with `apt` package manager, do
```
sudo apt install zenity
```

# Ramdisk Mountpoint

- Note: Not strictly necessary as the scripts will do this for you, but useful info if the path should be changed to a different location.

Example: Create the mountpoint `/media/ramdisk`
```
sudo mkdir -p /media/ramdisk
```
Change this directory to owner `<username>` by running `chown <username> /media/ramdisk` - substitute `<username>` for you username. This username also appears infront of the `mount` command - therefore `<username>` must be the same system user that runs the ramdisk mount script. (See the bash script `ramdiskinit` for more info.)

Change ownership: chown <username> /media/ramdisk - TODO: check if necessary

It is possible to change the mountpoint to a different location. The mount/free scripts contain a variable `RDPATH` where the ramdisk path can be set.

# fstab lines
Add the following line to `/etc/fstab` to allow user to mount a `tmpfs` system
```
tmpfs /media/ramdisk rw,nodev,nosuid,users,noauto,size=4G 0 0
```
Set the size of the ramdisk here. WARNING: 4G may be too large for some systems!

See also https://superuser.com/questions/1426285/how-can-i-mount-a-ramdisk-system-without-sudo-root/1426286#1426286

TODO: check if rw,nodev,nosuid,noexec are required/sensible

# Path variable
Path variable can be set in `.xsessionrc`. Add the following line to `~/.xsessiionrc`

```
export PATH=$PATH:/home/<username>/Programs/Ramdisk-Scripts
```
`<username>` must be the correct user - in other words, the scripts should be installed in the path `/home/<username>/Programs/Ramdisk-Scripts`

TODO: subfolder for Ramdisk-Scripts

# Optional: Create desktop launcher. (In this example, create an xfce4 launcher)

1. Menu->Settings->Settings Manager
2. Select Panel
3. Select desired panel
4. Select "Items"
5. Add new item, type Launcher
6. Repeat: Add new item, type Launcher

The first launcher will mount the ramdisk (call the mount script)

7. Select first launcher -> Edit

- Name: Start Ramdisk
- Comment: /media/ramdisk
- Command: `/home/<username>/Programs/Ramdisk-Scripts/ramdiskinit`
- Working Directory:
- Icon: (select disk icon or similar)
- Options:

The second launcher will unmount the ramdisk (call the unmount script)

8. Select second launcher -> Edit

- Name: Free Ramdisk
- Comment: /media/ramdisk
- Command: `/home/<username>/Programs/Ramdisk-Scripts/ramdiskfree`
- Working Directory:
- Icon: (select eject disk icon or similar)
- Options:


