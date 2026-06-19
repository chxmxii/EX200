# Managing Advanced Storage

## LVM :&#x20;

Logical Volume Manager was introduced to add flexibility to the partition, as it allow us to dynamically grow a partition that in running out of disk.\ <mark style="color:yellow;">Let's learn how to make an lvm partition togerther.</mark>\
as the picture below shows, to create an lvm partition you'll need first **physical volumes**, grouped in a **volume group**, then divided into **logical volumes**.

![](/files/N9UaaIDUhAuKaQYc70OH)

1. lsblk -> to select a free disk to make 2 partition with lvm type.&#x20;
2. fdisk /dev/sdc\
   2-1. n,primary,ENTER, ENTER, +1G, t,8e,p,w\
   2-2 n,primary,ENTER, ENTER, +1G, t,8e,p,w
3. lsblk again to verify the creation of /dev/sdc{1,2}
4. pvcreate /dev/sdc1 /dev/sdc2
5. pvdisplay
6. vgcreate myvg /dev/sdc1 /dev/sdc2 <mark style="color:green;">`-s 16 => if you want to set the PE.`</mark>
7. lvcreate -L 800M -n <mark style="color:yellow;">mylv</mark> <mark style="color:blue;">myvg</mark>&#x20;
8. mkfs.xfs /dev/mapper/myvg-mylv
9. mkdir /mnt/lvmp
10. blkid | grep myvg -> to get the UUID.
11. echo " UUID="..." /mnt/lvmp xfs defaults 0 0" >> /etfc/fstab
12. partx and then lsblk to verify.

If you wish to resize your lvm or your volume group do the following steps :&#x20;

1. <mark style="color:yellow;">To extend the volume group by adding new physical voume :</mark> \
   1-1. pvcreate /dev/sdc3\
   1-2. vgextend myvg /dev/sdc3&#x20;
2. <mark style="color:yellow;">To extend the logical volume :</mark> \
   2-1. lvextend -L +100M /dev/myvg/mylv \
   2-2. xfs\_growfs /dev/myvg/mylv => xfs \
   &#x20;       resize2fs /dev/myvg/mylv => ext.\
   2-3. lvresize -r -L +100M /dev/myvg/mylv.
3. <mark style="color:yellow;">To reduce the logical volume :</mark> \
   3-1. lvreduce -r -L -500M /dev/myvg/mylv
4. <mark style="color:yellow;">to create a snapshot :</mark>  \
   4-1. lvcreate -s -L 150M -n mysnap /dev/myvg/mylv\
   4-2. mount it to use it.\
   4-3. to destroy the snapshot you'll have to umount it and then \
   &#x20;       lvremove /dev/myvg/mysnap.

## Stratis :&#x20;

Stratis is the next generation local storage manager. it was introduced in RHEL8 and it uses xfs as the default file system. The amazing thing about stratis file system is auto extendable.\
If you understand how to use lvm, then you're good to start with stratis as both almost have the same technique.&#x20;

For a better understanding, check the Stratis technology with the diagram below :&#x20;

![](/files/Qq1PpKVtfZbeIiVaoEEP)

Moving forward now, lets learn how to use stratis.

1. rpm -qa | grep stratis -> to verify if its installed
2. yum install stratisd stratis-cli -y -> to install
3. systemctl enable --now stratis -> to enable the stratisd service
4. lsblk -> to select two block device to use, assuming we selected (sdd,sde)
5. use **wipefs -a /dev/\<device>** if it contains any partition.\
   or you can use : **shred -vfz -n 10 /dev/\<device>**
6. <mark style="color:blue;">stratis pool create mystratis\_pool /dev/sdd /dev/sde</mark>
7. <mark style="color:blue;">stratis pool list</mark> -> to verify the stratis pool is created.
8. &#x20;<mark style="color:blue;">stratis blockdev list</mark>  <mark style="color:yellow;">mystratis\_pool,</mark> then do lsblk to verify
9. <mark style="color:blue;">stratis fs create</mark> <mark style="color:yellow;">mystratis\_</mark>*<mark style="color:yellow;">pool</mark>* <mark style="color:red;">mystratis\_filesystem</mark>
10. <mark style="color:blue;">stratis fs list mystratis\_pool</mark>
11. mkdir /data -> to create mont point&#x20;
12. blkid | grep stratis\_pool and copy the UUID
13. echo " UUID=".."  /data xfs defaults,**x-systemd.requires=stratisd.service** 0 0" >> /etc/fstab
14. <mark style="color:blue;">mount -a; mount</mark> -> to verify
15. lsblk to verify
16. <mark style="color:blue;">stratis fs snapshot</mark> mystratispool mystratis\_fs mystratis\_snapshot
17. mkdir /snapshot ; mount /mystratis\_pool/mystratis\_fs/mystratis\_snapshot /snapshot
18. <mark style="color:blue;">stratis pool add-data mystratis\_pool /dev/sdf -></mark> if you want to extend the stratis pool.
19. <mark style="color:blue;">stratis fs destroy mystratis\_</mark>*<mark style="color:blue;">pool mystratis</mark>*<mark style="color:blue;">\_fs,</mark> requires umounting.
20. stratis pool init-cache \<poolname> /dev/\<devicename> \
    \=> to add cache to pool, cache will collect the most used data for you.

## VDO :&#x20;

Virtual Data Optimizer, Another advanced storage solution offered in rhel8.\
Focuses on storages the data in the most efficient way, with the concept of deduplicating and compressed storage tools. \
Mainly used in cloud and containerized environment, and provides a thin-provisioned.\
You can take a look at the diagram below for better understanding.

![](/files/hZsRG7vYdw9LQitpPJve)

* yum install <mark style="color:blue;">vdo kmod-kvdo</mark> -> to install vdo service.
* <mark style="color:blue;">vdo</mark> create <mark style="color:red;">--name=vdo1</mark> --device=/dev/sda --vdoLogicalSize=1T ->to create one.
* <mark style="color:blue;">mkfs.xfs -K /dev/mapper/vdo1</mark> (-k = do not attempt to discard blocks).
* <mark style="color:blue;">udevadm settle</mark> -> to regesiter the new device in the kernel.
* <mark style="color:blue;">mkdir /vdo-data</mark> -> to create a mountpoint.
* <mark style="color:blue;">lsblk --output=UUID /dev/mapper/vdo1</mark> -> to get the UUID.
* <mark style="color:blue;">vim /etc/fstab</mark> and insert this line. \ <mark style="color:yellow;">UUID="..." /vod-data xfs defaults,x-systemd.requires=vdo.service 0 0"</mark>&#x20;
* <mark style="color:blue;">mount -a</mark> -> to verify mounting.
* <mark style="color:blue;">vdostats --hu.</mark>

{% hint style="info" %}
You can also mount the vdo using systemd.mounts. Find an example within /usr/share/doc/vdo/examples.
{% endhint %}

{% hint style="warning" %}
if you had an error while creating \<vdo : ERROR - Found an existing signature on /dev/sdb  at offset 512> \
-> this is safety check, telling you the volume seems to be already \
initialized, as possibly used. \
\=> to get rid of this error, simply insert this command line syntax : \
&#x20; **wipefs --all --force /dev/sdb**
{% endhint %}

## LUKS :&#x20;

Linux Unified Key Setup, is a disk encryption specification created in 2004 by Clemens Fruhwirth in 2004. \
To setup a LUKS encrypted volume :&#x20;

* Create partition with parted.
* crypetsetup luksFormat \<devicename> -> to format the luks device.
* crypetsetup luksOpen \<devicename> \<device\_mapper\_name>
* mkfs.xfs \<device\_*mapper*\_name> -> to format the device partition.
* mkdir /encrypted
* echo "/dev/mapper/\<DEVICE> /encrypted xfs defaults 0 0" >> /etc/fstab.&#x20;
* crypetsetup luksAddKey \<device\_name> /home/secure.txt
* echo "/encrypted /dev/\<devicename> /home/secure.txt" >> /etc/cryptab.\ <mark style="color:blue;">**CRYPTTAB :**</mark> crypttab describes encrypted block devices that are setup during system boot During boot, system will ask for password to mount /dev/mapper/myvol on /test1 directory. To setup automatic mount without password, add a key file for /dev/mapper/myvol. This has to be in /etc/crypttab as well.\
  FORMAT : <mark style="color:blue;">mount point</mark> <mark style="color:red;">partition name</mark> <mark style="color:green;">path to secretphrase.</mark>
