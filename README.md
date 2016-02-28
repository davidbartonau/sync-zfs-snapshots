# sync-zfs-snapshots
Synchronises all snapshots from one zfs filesystem to another.

This is useful if you want to have a set of backup USBs and you want them to have all your zfs snapshots or if you have a remote server that a sync may fail.  

Simply run sync-zfs-snapshots and it will work out all the missing snapshots and send them in suitable order.  It's designed to be very easy to use.

#### Syntax
````
sync-zfs-snapshots [--sshIdentity /root/.ssh/id_rsa_backup] <source filesystem> <target filesystem>
````

Source or destination filesystem can be either:
- local mounted zfs filesystem: \<zpool>/\<filesystem>
- zfs filesystem on a remote server via ssh: ssh://\<username>@\<server>[:port]:\<zpool>/\<filesystem>

#### Example
````
sync-zfs-snapshots --sshIdentity /root/.ssh/id_rsa_backup ssh://root@my.server.com:2022:backups/current backups_usb001/current
````

## Setup
Let's say you have a filesystem mypool/files and you want to replicate to another disk on the same server.  First set up the new pool and the target filesystem.
````
zpool create -o ashift=12 otherpool /dev/xvda12
zfs create otherpool/files2
````

Now we replicate any snapshots from mypool/files into the target.  It doesn't matter if there are 2 or 200 snapshots to replicate.
````
sync-zfs-snapshots mypool/files otherpool/files2
````
