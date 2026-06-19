# Storage Management

Before starting, make sure to read about MBR (Master Boot Record) and GPT (GUID Partition Table).

## Disk Devices and MBR vs GPT

List of disk device types:

- `/dev/sda` - KVM disk device type
- `/dev/nvme0n1` - SSD device type
- `/dev/hda` - IDE disk device type
- `/dev/xvda` - Xen virtual disk driver

How to create a backup for a disk:

```bash
dd if=/dev/<device> of=/root/diskfile bs=1M count=1
```

Difference between MBR and GPT:

- **MBR:** Cannot create more than 4 partitions, cannot partition disks larger than 2TB.
- **GPT:** Supports 128+ partitions, can partition disks up to 8ZB.

Use `parted -l` to know which partition table your OS uses (`msdos` means MBR).

## Creating Partitions

There are many different ways to create a partition, either using `fdisk`, `gdisk`, or `parted`. First you need to list all the block devices to know which device is available using `lsblk`.

### fdisk (MBR)

To create a partition using `fdisk`, follow these steps:

```bash
fdisk /dev/sda
```

1. `m` - to list all the commands
2. `n` - to create new partition
3. Select `primary` to make it a primary partition
4. Press ENTER to set the first sector
5. Specify the partition size, e.g. `+500M`
6. `p` to print the table
7. `w` to save
8. `partprobe` to inform the kernel that a new device partition is added

### gdisk (GPT)

To create a partition using `gdisk`:

```bash
gdisk /dev/sdb
```

1. `?` - to list all the commands
2. `n` - to create new partition
3. Press ENTER to leave the first sector by default
4. Specify the disk partition size (if you leave it empty the whole disk will be partitioned), e.g. `+500M`
5. Press ENTER to leave the partition type at Linux file system
6. `p` - to print the partition table
7. `w` to save
8. Use `partprobe` or `partx` to tell the kernel a new partition is added

### parted (Both)

You can create a partition using `parted`, which supports both MBR and GPT:

```bash
parted /dev/sdc
```

1. `mklabel` (gpt/msdos)
2. `mkpart`
   - primary
   - filesystem type (xfs, ext4, ext3, etc.)
   - starting from: `1MiB` (if you already have a partition ending at 154MiB, start from 155MiB)
   - ends in: `257MiB`
3. `set`
   - Partition Number: 1
   - Flag to invert: to set the partition type
   - New state: on
4. `quit`

## Filesystems

A filesystem is a way of arranging files on a drive. `xfs` is the default file system for RHEL 8. Previous versions used `ext` (2, 3, 4) as the default.

### Creating Filesystems

Use `fsck -N <partition>` to determine the file system of a particular partition.

Use `mkfs.<type> /dev/<devicename>` to create a file system for a partition:

```bash
mkfs.xfs /dev/sda
```

### Managing Filesystems

For `ext` filesystems:

```bash
tune2fs -l /dev/sdx               # show filesystem properties
tune2fs -o acl,user_xattr /dev/sdx # enable ACL and user_xattr
tune2fs -o ^acl,user_xattr /dev/sdx # disable ACL and user_xattr
tune2fs -L label /dev/sdb          # create a label for /dev/sdb
```

For `xfs` filesystems:

```bash
xfs_admin -L label /dev/sdb   # create a label for /dev/sdb
xfs_admin -l /dev/sdx         # show xfs properties
```

## Mounting Filesystems

Use `df -hT` to see all the drives and partitions mounted.

Steps to mount a file system:

```bash
mkdir /mnt/mymp                                        # create a mountpoint
mount /dev/<devicename> /mnt/mymp                      # mount temporarily (can use LABEL instead)
echo "/dev/sda /mnt/mymp xfs defaults 0 0" >> /etc/fstab  # mount persistently
systemctl daemon-reload
mount -a                                               # mount all entries in fstab
mount                                                  # verify the mounting
umount /mnt/mymp                                       # unmount the device
```

## Swap Partitions

Swap partition is used to support the memory. Below are the steps to make a 200M swap partition:

```bash
free -m                  # check memory state
lsblk                   # see which disk is free to work on
```

1. Create the partition:

```bash
fdisk /dev/sdc
# n, ENTER, ENTER, +200M, t (type), L (list, search for swap), 82, w
```

2. Verify and activate:

```bash
partx -a /dev/sdc
lsblk                                                # verify partition creation
mkswap /dev/sdc1                                     # make swap on the partition
echo "/dev/sdc1 swap swap defaults 0 0" >> /etc/fstab # mount the swap persistently
swapon /dev/sdc1                                     # activate the swap
free -m                                              # verify
```

## LVM (Logical Volume Manager)

Logical Volume Manager was introduced to add flexibility to partitioning, as it allows you to dynamically grow a partition that is running out of disk space. To create an LVM partition you need **physical volumes**, grouped in a **volume group**, then divided into **logical volumes**.

### Creating LVM

1. Select a free disk and create partitions with LVM type:

```bash
lsblk
fdisk /dev/sdc
# n, primary, ENTER, ENTER, +1G, t, 8e, p, w
# n, primary, ENTER, ENTER, +1G, t, 8e, p, w
```

2. Verify and create physical volumes:

```bash
lsblk                              # verify /dev/sdc1 and /dev/sdc2 exist
pvcreate /dev/sdc1 /dev/sdc2
pvdisplay
```

3. Create volume group:

```bash
vgcreate myvg /dev/sdc1 /dev/sdc2
# Use -s 16 if you want to set the PE size, e.g.: vgcreate -s 16 myvg /dev/sdc1 /dev/sdc2
```

4. Create logical volume:

```bash
lvcreate -L 800M -n mylv myvg
```

5. Format and mount:

```bash
mkfs.xfs /dev/mapper/myvg-mylv
mkdir /mnt/lvmp
blkid | grep myvg                  # get the UUID
echo "UUID=\"...\" /mnt/lvmp xfs defaults 0 0" >> /etc/fstab
systemctl daemon-reload
mount -a
lsblk                              # verify
```

### Extending and Reducing Volumes

To extend the volume group by adding a new physical volume:

```bash
pvcreate /dev/sdc3
vgextend myvg /dev/sdc3
```

To extend the logical volume:

```bash
lvextend -L +100M /dev/myvg/mylv
xfs_growfs /dev/myvg/mylv          # for xfs
resize2fs /dev/myvg/mylv           # for ext
# Or use lvresize with -r to resize filesystem automatically:
lvresize -r -L +100M /dev/myvg/mylv
```

To reduce the logical volume:

```bash
lvreduce -r -L -500M /dev/myvg/mylv
```

### Snapshots

To create a snapshot:

```bash
lvcreate -s -L 150M -n mysnap /dev/myvg/mylv
```

Mount the snapshot to use it:

```bash
mkdir /mnt/snap
mount /dev/myvg/mysnap /mnt/snap
```

To destroy a snapshot, unmount it first:

```bash
umount /mnt/snap
lvremove /dev/myvg/mysnap
```

## Stratis

Stratis is the next generation local storage manager. It was introduced in RHEL 8 and uses `xfs` as the default file system. Stratis file systems are auto-extendable. If you understand how to use LVM, then Stratis uses a similar technique.

Steps to configure Stratis:

1. Install and enable Stratis:

```bash
rpm -qa | grep stratis              # verify if installed
yum install stratisd stratis-cli -y # install
systemctl enable --now stratisd     # enable the stratisd service
```

2. Prepare block devices:

```bash
lsblk                              # select block devices (e.g. sdd, sde)
wipefs -a /dev/<device>            # remove existing signatures
# Or: shred -vfz -n 10 /dev/<device>
```

3. Create pool and filesystem:

```bash
stratis pool create mystratis_pool /dev/sdd /dev/sde
stratis pool list                  # verify pool creation
stratis blockdev list mystratis_pool
stratis fs create mystratis_pool mystratis_filesystem
stratis fs list mystratis_pool
```

4. Mount the filesystem:

```bash
mkdir /data                        # create mount point
blkid | grep stratis_pool          # get the UUID
echo "UUID=\"...\" /data xfs defaults,x-systemd.requires=stratisd.service 0 0" >> /etc/fstab
mount -a
mount                              # verify
lsblk                              # verify
```

5. Create a snapshot:

```bash
stratis fs snapshot mystratis_pool mystratis_fs mystratis_snapshot
mkdir /snapshot
mount /dev/stratis/mystratis_pool/mystratis_snapshot /snapshot
```

6. Extend the pool:

```bash
stratis pool add-data mystratis_pool /dev/sdf
```

7. Destroy a filesystem (requires unmounting first):

```bash
umount /data
stratis fs destroy mystratis_pool mystratis_fs
```

8. Add cache to pool:

```bash
stratis pool init-cache <poolname> /dev/<devicename>
# Cache collects the most frequently used data for faster access
```

## VDO (Virtual Data Optimizer)

Virtual Data Optimizer is an advanced storage solution offered in RHEL 8. It focuses on storing data in the most efficient way, with deduplication and compression. Mainly used in cloud and containerized environments, VDO provides thin-provisioned storage.

Steps to configure VDO:

```bash
yum install vdo kmod-kvdo                                    # install VDO
vdo create --name=vdo1 --device=/dev/sda --vdoLogicalSize=1T # create VDO volume
mkfs.xfs -K /dev/mapper/vdo1                                 # format (-K = do not discard blocks)
udevadm settle                                               # register new device in the kernel
mkdir /vdo-data                                              # create mountpoint
lsblk --output=UUID /dev/mapper/vdo1                         # get the UUID
```

Add to `/etc/fstab`:

```bash
echo "UUID=\"...\" /vdo-data xfs defaults,x-systemd.requires=vdo.service 0 0" >> /etc/fstab
mount -a                                                     # verify mounting
vdostats --hu                                                # check VDO statistics
```

Note: You can also mount VDO using `systemd.mount` units. Find an example within `/usr/share/doc/vdo/examples`.

If you get an error while creating VDO (`ERROR - Found an existing signature on /dev/sdb at offset 512`), this is a safety check telling you the volume seems to be already initialized. To resolve:

```bash
wipefs --all --force /dev/sdb
```

## LUKS Encryption

Linux Unified Key Setup (LUKS) is a disk encryption specification created by Clemens Fruhwirth in 2004.

To set up a LUKS encrypted volume:

1. Create a partition with `parted`
2. Format and open the LUKS device:

```bash
cryptsetup luksFormat <devicename>
cryptsetup luksOpen <devicename> <device_mapper_name>
```

3. Create filesystem and mount:

```bash
mkfs.xfs /dev/mapper/<device_mapper_name>
mkdir /encrypted
echo "/dev/mapper/<DEVICE> /encrypted xfs defaults 0 0" >> /etc/fstab
```

4. Add a key file for automatic unlocking:

```bash
cryptsetup luksAddKey <device_name> /home/secure.txt
echo "<device_mapper_name> /dev/<devicename> /home/secure.txt" >> /etc/crypttab
```

The `/etc/crypttab` file describes encrypted block devices that are set up during system boot. During boot, the system will ask for a password to mount the encrypted device. To set up automatic mounting without a password, add a key file. The `crypttab` format is:

```
<mapper_name> <partition> <path_to_key_file>
```
