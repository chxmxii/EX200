# Storage Management

Not going deeply in this lesson.\
Before starting, make sure to read about MBR ''Master Boot Record" and GPT "GUID Partition Table".\
Lets go... \ <mark style="color:yellow;">1- List of disk device types :</mark>\
&#x20;  -> /dev/sda -> KVM disk device type <----> /dev/nvme0n1 -> SSD device type\
&#x20;  -> /dev/hda -> IDE disk device type   <----> /dev/xvda -> Xen virtual disk driver.\ <mark style="color:yellow;">2- How to create a backup for a disk</mark>\
&#x20;  -> dd if=/dev/\<device> of=/root/diskfile bs=1M count=1\ <mark style="color:yellow;">3-Difference between MBR and GPT :</mark> \
&#x20;  -> <mark style="color:blue;">**MBR :**</mark> Can't create more than 4 partition, can't partition the disk with +'2TB.  \
&#x20;  -> <mark style="color:blue;">**GPT :**</mark> +128 partition, can partition the disk +8ZB \ <mark style="color:green;">-> parted -l :</mark> To know which one your OS uses. (msdos is MBR).\ <mark style="color:yellow;">4-List of storage units :</mark>&#x20;

![](/files/NFJcq1hDJC5DTUB0BRmg)

## Creating a standard partition : &#x20;

There is many different ways to create a partition, either using fdisk, gdisk or parted.\
First you'll need to list all the blocks to know which device is avaible using **"lsblk"**\
Lets start using fdisk utility.

### fdisk  (MBR) :&#x20;

to create a partition using fdisk follow my steps : \
-> fdisk /dev/sda\
&#x20; 2- m : to list all the commands\
&#x20; 3- n : to create new partition\
&#x20; 4- primary : to make it a primary partition, \
&#x20; 5- PRESS ENTER to set the first sector\
&#x20; 6- specify the partition size, exp +500M\
&#x20; 7- p to print the table.\
&#x20; 8- w to save.\
&#x20; 9- partprobe to informe the kernel that a new device partition is added.&#x20;

### gdisk (GPT) :&#x20;

Lets create a partition using gdisk : \
-> gdisk /dev/sdb\
&#x20;2- ? : to list all the commands\
&#x20;3- n : to create new partition \
&#x20;4-PRESS ENTER : to leave the first sector by default\
&#x20;5- Specify the disk partition size, if you leave it empty the whole disk will partitioned.\
&#x20; <mark style="color:red;">exp :</mark> +500M\
&#x20;6- PRESS ENTER : to leave the partition type at Linux file system.\
&#x20;7- P : to print the partition table.\
&#x20;8- w to save.\
&#x20;9-  use **partprobe - partx** to tell the kernel new partition is added.

### Parted (BOTH) :&#x20;

You can create a partition using parted, parted support mbr and gtp.\
-> parted /dev/sdc\
&#x20; 2- mklabel (gpt/msdos)\
&#x20; 3- mkpart \
&#x20;   3-1 primary \
&#x20;   3-2 filesystemtype (xfs-ext4-ext3..)\
&#x20;   3-3 starting from : 1MiB --- <mark style="color:blue;">`Assuming we have already a partition with 154MiB, then you will need to start from 155MiB.`</mark>\
&#x20;   3-4 ends in : 257 MiB.\
&#x20; 4- set \
&#x20;   4-1 Partition Number : 1 \
&#x20;   4-2 flag to invert : to set the partition type.\
&#x20;   4-3 New state? on\
&#x20; 5-quit.&#x20;

## file system :&#x20;

<mark style="color:blue;">You don't know what is file system?</mark> \
\
A filesystem in this context is a way of arranging, or rather, a system of arranging files on a drive. Just as books can be arranged in a shelve horizontally or vertically, a filesystem is also a system files can be arranged on a device.\
xfs is the default file system for rhel8, pervious versions, ext(2,3,4) were the default one.&#x20;

<mark style="color:yellow;">fsck -N \<partition></mark> to know the file system of a particular partition.\ <mark style="color:yellow;">mkfs.\<type> /dev/\<devicename></mark> to create a file system for a partition.\ <mark style="color:green;">example :</mark> mkfs.xfs /dev/sda

#### Managing file system :&#x20;

⦁ ext: \
&#x20;  <mark style="color:red;">Tune2fs -l</mark> /dev/sdx  -> showing filesystem properties.\
&#x20;  <mark style="color:red;">Tune2fs -o</mark> acl,user\_xattr | tune2fs -o ^acl,user\_xattr (to switch it off). \
&#x20; <mark style="color:red;">Tune2fs -L</mark> label /dev/sdb -> to create a label for /dev/sdb.\
&#x20;⦁ Xfs:  \
&#x20;  <mark style="color:green;">xfs-admin -L</mark> label /dev/sdb  to create a label for /dev/sda.\
&#x20;   <mark style="color:green;">xfs\_admin -l</mark> /dev/sdx  -> to show xfs properties.

## Mounting file system :&#x20;

Below, you will find the write steps to mount a file system : \
-> <mark style="color:red;">df -hT</mark> to all the drivers and parts mounted on.\
&#x20;1- <mark style="color:yellow;">mkdir /mnt/mymp</mark> to create a mountpoint\
&#x20;2- <mark style="color:blue;">mount</mark> /dev/\<devicename> <mark style="color:red;">\<mount point></mark> (You can use LABEL instead of dname)\
&#x20; <mark style="color:green;">exp :</mark> mount /dev/sda /mnt/mymp \
\=> to mount the device temporarily \
&#x20;3- <mark style="color:blue;">echo</mark> "/dev/sda  /mnt/mytmp  xfs  defaults 0 0" >> <mark style="color:blue;">/etc/fstab</mark> \
\=> to mount it persistently.\
&#x20;4- <mark style="color:yellow;">systemctl daemon-reload</mark>\
&#x20;5- <mark style="color:yellow;">mount -a</mark> \
&#x20;6- <mark style="color:yellow;">mount</mark>\
\=> to verify the  mounting.\
&#x20; 7- <mark style="color:yellow;">umount /mnt/mymp</mark> to unmount the device.

### Creating a swap partition :&#x20;

swap partition is used to support the memory. below you will find the right steps to make a 200M swap partition :&#x20;

1. free -m -> to check kthe memory states.\
   lsblk -> to see what disk is free to work on
2. fdisk /dev/sdc (n,ENTER,ENTER,200M,t(type),L(list)(search for swap),82,w)
3. partx; lsblk -> to verify the creation
4. mkswap /dev/sdc -> to make swap on /dev/sdc
5. echo "/dev/sdc swap swap defaults 0 0" >> /etc/fstab -> to mount the swap
6. swapon /dev/sdc to activate the swap
7. free -m -> to verify.

{% hint style="info" %}
Hope this was useful, lets advance and learn about lvm and stratis and vdo.
{% endhint %}
