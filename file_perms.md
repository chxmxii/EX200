# Ownership & Permissions

## Ownership :

To modify the ownership of (file/folder) use :&#x20;

* <mark style="color:red;">chown</mark> <mark style="color:blue;">\<user></mark>:<mark style="color:yellow;"><mark style="color:blue;">\<group><mark style="color:blue;"></mark> <mark style="color:yellow;"></mark><mark style="color:yellow;"><mark style="color:green;">\</filename><mark style="color:green;"></mark>
* <mark style="color:red;">chgrp</mark> to change the group owner only.

After using these commands, use ls -l to ensure the changes.&#x20;

## Permissions :&#x20;

Lets understand what are the permissions and what is their affect on files and directories.

| Permissions | Files  | Directories   |
| ----------- | ------ | ------------- |
| Read (4)    | Read   | List          |
| Write (2)   | Modify | Delete/create |
| Execute (1) | Run    | CD            |

Now after understanding the permissions lets learn how to give permissions to files and directories using the <mark style="color:yellow;">chmod</mark> command.

* <mark style="color:red;">chmod</mark> 770 <mark style="color:blue;">\<f/d\_name></mark> -> we gave full permissions to user and group and nothing to others. 7 = 4+2+1 = rwx.
* <mark style="color:red;">chmod</mark> u=rwx, g=rwx, o=r  <mark style="color:blue;">\<f/d\_name></mark> -> Same thing but in another format and we gave the "others" read permission.
* <mark style="color:red;">chmod</mark> a+x <mark style="color:blue;">\<f/d\_name></mark> -> we gave the execution permission for all.
* <mark style="color:red;">chmod</mark> -R 750 <mark style="color:blue;">\<PATH></mark> -> The "-R" option will grant the given permission recursively.
* <mark style="color:red;">chmod</mark> ug+rw <mark style="color:blue;">\<filename></mark> -> we gave user and group owner the rw perm.

### umask :&#x20;

lets make it simple and define umask in simpe english, <mark style="color:yellow;">umask</mark> is used to set the file permissions for newly created file.&#x20;

{% hint style="info" %}
Default file permissions is : 666

Default directory permissions is : 777
{% endhint %}

* <mark style="color:red;">umask 052</mark> -> as we've said above the file default permissions is 666 so if we run this command the file permissions for the newly created file will be 614 (666-022) same thing for directories, the new permission will set 725 (777-052).

using umask command is temporary, to make it persistent edit the umask in this files :&#x20;

1. vim \~/.bashrc&#x20;
2. vim \~/.bash\_profile -> to change umask for a specific user.
3. vim /etc/profile -> to change umask for all users.
4. vim /etc/login.defs -as a root user.
5. add <mark style="color:yellow;">umask.sh</mark> under /etc/profile.d.

### Special Permissions :&#x20;

The utility of special permissions is very simple lemme explain it for you.

|   Permissions  |        File        |                  Directory                  |
| :------------: | :----------------: | :-----------------------------------------: |
|    SUID (4)    |    Run as owner    |                     N/A                     |
|     SGID(2)    | Run as group owner | <p>inherit directory </p><p>group owner</p> |
| Sticky bit (1) |         N/A        |             Delete only if owner            |

Just to make it more simpler, adding the Set-User-ID will permit you to run the file with the owner permission.&#x20;

Now that we understand the special permissions lets learn how to use them.

* <mark style="color:red;">chmod</mark> <mark style="color:yellow;">u+s</mark> \<filename> | <mark style="color:red;">chmod</mark> <mark style="color:blue;">4</mark>750 \<filename> -> adding SUID permission.
* <mark style="color:red;">chmod</mark> <mark style="color:yellow;">g+s</mark> \<f/d\_name> | <mark style="color:red;">chmod</mark> <mark style="color:blue;">2</mark>750 \<filename> -> adding SGID permission.
* <mark style="color:red;">chmod</mark> <mark style="color:yellow;">+t</mark> \<dirname> | <mark style="color:red;">chmod</mark> <mark style="color:blue;">1</mark>750 \<dirname> -> adding the Sticky bit permission.

### ACLs :&#x20;

Access Control List is used to give a specific permission to a specific user or group or others.

Use : &#x20;

* <mark style="color:red;">getfacl</mark> <mark style="color:blue;"><mark style="color:yellow;">\<f/d\_name><mark style="color:yellow;"></mark> -> to get the file access control list of \<filename>.
* <mark style="color:red;">setfacl</mark> <mark style="color:green;">-R</mark> <mark style="color:yellow;">-m</mark> <mark style="color:blue;">d</mark> g:redhat:rw- <mark style="color:blue;">\</webapp/myapp></mark> -> **-R** refers to recursively, **-m** refers to modify, **d** refers to default. this command will give the redhat group the rw permissions recursively and by default for <mark style="color:blue;">**\</webapp/myapp>.**</mark>

### Attributes :&#x20;

File attributes are extra features you can use to tune a given file.

* <mark style="color:red;">lsattr</mark> \<filename> -> to list the attributes of the \<filename>.
* <mark style="color:red;">chattr</mark> \<filename> -> to change the attributes of the \<filename>.

Below is a summary of the most common attributes:

* <mark style="color:blue;">**A**</mark> -> When the file is accessed the atime is not update. Good for minimizing disk I/O on a laptop.
* <mark style="color:blue;">**a**</mark> -> When this file is opened, it is opened in append only mode for writing.
* <mark style="color:blue;">**c**</mark> -> This file is automatically compressed on the disk by the kernel.
* <mark style="color:blue;">**i**</mark> -> This file cannot be modified, renamed or deleted.

### Practice Time :&#x20;

Now after we are done with explanation. lets do a small lab.\
-) as a root user create a file with name script under directory redhat. \
-) make redhat the group owner of this directory and chxmxii the owner.\
-)the owner of Redhat directory will have full permission while the group owner will have read and write only, others will have nothing. Also ensure that only the owner can delete the script file .\
-)the group students will have the read and write permission on this file.\
-)change the attributes so the script file cannot be modified, renamed or deleted.

#### Answer :&#x20;

```
mkdir /redhat; touch /redhat/script
chown -R chxmxii:redhat /redhat/
chmod -R 1760 /redhat/
setfacl -R -m d g:students:rw- /redhat/
chattr +i /redhat/script 
```
