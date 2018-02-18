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

#step 3
Remove Old Docker
Pre-Reqs for Media Box (https://github.com/phikai/mediabox#installation)
Install Docker
https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository
Install Docker Compose
https://docs.docker.com/compose/install/#install-compose

