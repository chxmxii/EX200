# Remote Filesystem

## NFS :&#x20;

<mark style="color:blue;">NFS</mark>, which is an acronym for (Network File system) is a distributed file system protocol.\
NFS is server-client based, where the NFS server will be configured and clients can access files on the server over a network just as if they were on the server.

Step by step am going to show you how to use the nfs share.\
Assuming we want to share the directory /database from server 1 to server2\
\ <mark style="color:red;">**SERVER 1 :**</mark> \
**IP ; 192.168.222.131**

1. systemctl enable --now nfs-server.service
2. systemctl status nfs-server.service
3. vim /etc/export \
   \>>/database 192.168.222.129(rw,sync,no\_root\_squash)\
   save and quit
4. firewall-cmd --add-service={rpc-bind,nfs,mountd} --permanent
5. firewall-cmd --reload
6. exportfs -arv

Moving to \ <mark style="color:red;">**SERVER 2 :**</mark> \
**IP; 192.168.222.129**

1. systemctl enable --now nfs-server.service
2. systemctl status nfs-server.service
3. firewall-cmd --add-service={rpc-bind,nfs,mountd} --permanent
4. firewall-cmd --reload
5. showmount -e 192.168.222.131
6. mkdir /database-mp
7. echo "192.168.222.131:/database /database-mp nfs \_netdev 0 0" >> /etc/fstab.
8. mount -a

## **CILFS :**&#x20;

![SERVER ](/files/3UsK5oQ6jhz3OGUQT4IR)

![CLIENT ](/files/uBpB02UXv0MSjgZIn5Dm)

`Troubleshooting:` [`https://wiki.samba.org/index.php/Troubleshooting_SELinux_on_a_Samba_AD_DC`](https://wiki.samba.org/index.php/Troubleshooting_SELinux_on_a_Samba_AD_DC)

## Autofs :&#x20;

Lazy load volumes when they are needed, not simply at boot-time with `fstab`.

* Install the `autofs` package
* `/etc/auto.master` defines the directory and mount options file ex: `/data /etc/auto.data`
* `/etc/auto.data` defines the sub-directory (within `/data`) and how to mount the thing ex: `files -rw nfs.evilcorp.com:/data/files`
* Start `autofs` service
* Automount when started will auto create `/misc` and `/net`, which it uses
* There are great examples in `/etc/auto.misc`
* Automount will auto unmount idle volumes
