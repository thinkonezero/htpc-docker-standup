# htpc-docker
My HTPC Docker Configuration


#Step 1
Install Ubuntu Server
`sudo apt update` then `sudo apt upgrade`

#Step 2
Mount NTFS Drive w/ Appropriate Permissions
https://askubuntu.com/questions/113733/how-do-i-correctly-mount-a-ntfs-partition-in-etc-fstab#comment684545_113746
https://help.ubuntu.com/community/Fstab#ntfs

`sudo blkid` - Find the block ID of your media drive
`sudo mkdir /media/Media` - make the mount point
Add to `/etc/fstab`: `UUID=BC6617A366175D88 /media/Media ntfs-3g uid=1000,gid=1000,dmask=022,fmask=133 0 0`

#Step 3
Setup Samba Share (to access media outside of HTPC)
https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated%2C%20Simple%20and%20Brief%20Way%21

#step 3
Remove Old Docker
Pre-Reqs for Media Box (https://github.com/phikai/mediabox#installation)
Install Docker
https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository
Install Docker Compose
https://docs.docker.com/compose/install/#install-compose


This thread is GOLD: https://lime-technology.com/forums/topic/44108-support-binhex-general/?tab=comments#comment-433613

Removing Old Torrents: https://www.cuttingcords.com/home/2015/2/4/auto-deleting-finished-torrents-from-deluge

Sonarr Series Folders: https://forums.sonarr.tv/t/adding-new-series-path-issues/2751/2

Encoding of the Portainer Password (with auth): https://github.com/portainer/portainer/issues/1506
