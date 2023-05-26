

# Disks
How to mount disks on startup is controlled by the `\etc\fstab` file. 
* Mounting NFS share (which I use to mount my NAS at home) [here](https://linuxize.com/post/how-to-mount-an-nfs-share-in-linux/)
Once you've made changes to the `fstab` file you can re-mount everything with `mount -a`

## Mount sftp disk in nautillus
To mount a disk in nautilus we go to "other locations" and then add
```
sftp://nvidia@carter-v23-9/home/nvidia
```
