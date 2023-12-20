
## Access
To access via the web interface go to: https://find.synology.com/
user: `alex`
pass: `<new secure pass>`


## Setting up
Notes I made while setting up my NAS

* Synology on linux mapped folders [here](https://kb.synology.com/en-us/DSM/tutorial/How_to_access_files_on_Synology_NAS_within_the_local_network_NFS)
* How to mount an nsf share in linux [here](https://linuxize.com/post/how-to-mount-an-nfs-share-in-linux/)

Then edit the `/etc/fstab` to mount the folder on startup. My `fstab` looks like

```bash
alex@mildinsight:~$ cat /etc/fstab 
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/nvme0n1p2 during installation
UUID=9e272d55-59ad-4bd9-9bc4-a9408c974518 /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/nvme0n1p1 during installation
UUID=3754-5488  /boot/efi       vfat    umask=0077      0       1
/swapfile                                 none            swap    sw              0       0
# NAS - datasets folder
192.168.1.52:/volume1/datasets /media/alex/nas/datasets/ nfs rw,user,exec 0 0
# NAS - shallowinsight trunk backup folder
192.168.1.52:/volume1/shallowinsight_backup /media/alex/nas/shallowinsight_trunk_backup/ nfs rw,user,exec 0 0
```

After you make changes to the fstab you can reload it so they take effect with:
```bash
mount -av
```

## Permissions
I had issues with the folders under media not getting mounted with permissions to allow me to read/write. 

Just change the permissions on the folder (you need to do it after the folder is mounted)
```bash
sudo chmod +777 shallowinsight_trunk_backup/
```
