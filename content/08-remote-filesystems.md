# Remote Filesystems

## NFS

NFS, which is an acronym for Network File System, is a distributed file system protocol.
NFS is server-client based, where the NFS server will be configured and clients can access files on the server over a network just as if they were on the server.

Step by step, here is how to use an NFS share.
Assuming we want to share the directory `/database` from server 1 to server 2.

**SERVER 1:**
IP: `192.168.222.131`

1. `systemctl enable --now nfs-server.service`
2. `systemctl status nfs-server.service`
3. `vim /etc/exports`
   ```bash
   /database 192.168.222.129(rw,sync,no_root_squash)
   ```
   Save and quit.
4. `firewall-cmd --add-service={rpc-bind,nfs,mountd} --permanent`
5. `firewall-cmd --reload`
6. `exportfs -arv`

**SERVER 2:**
IP: `192.168.222.129`

1. `systemctl enable --now nfs-server.service`
2. `systemctl status nfs-server.service`
3. `firewall-cmd --add-service={rpc-bind,nfs,mountd} --permanent`
4. `firewall-cmd --reload`
5. `showmount -e 192.168.222.131`
6. `mkdir /database-mp`
7. Add the following to `/etc/fstab`:
   ```bash
   192.168.222.131:/database /database-mp nfs _netdev 0 0
   ```
8. `mount -a`

## CIFS

Troubleshooting: [Troubleshooting SELinux on a Samba AD DC](https://wiki.samba.org/index.php/Troubleshooting_SELinux_on_a_Samba_AD_DC)

## Autofs

Lazy load volumes when they are needed, not simply at boot-time with `fstab`.

- Install the `autofs` package
- `/etc/auto.master` defines the directory and mount options file, e.g.: `/data /etc/auto.data`
- `/etc/auto.data` defines the sub-directory (within `/data`) and how to mount the share, e.g.: `files -rw nfs.evilcorp.com:/data/files`
- Start the `autofs` service
- Automount will auto-create `/misc` and `/net` when started, which it uses
- There are great examples in `/etc/auto.misc`
- Automount will auto-unmount idle volumes
