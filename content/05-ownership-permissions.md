# Ownership and Permissions

## Ownership

To modify the ownership of a file or folder use:

- `chown <user>:<group> <filename>`
- `chgrp` to change the group owner only.

After using these commands, use `ls -l` to ensure the changes.

## Permissions

Let's understand what the permissions are and what their effect is on files and directories.

| Permissions | Files  | Directories   |
| ----------- | ------ | ------------- |
| Read (4)    | Read   | List          |
| Write (2)   | Modify | Delete/create |
| Execute (1) | Run    | CD            |

Now after understanding the permissions let's learn how to give permissions to files and directories using the `chmod` command.

- `chmod 770 <f/d_name>` — we gave full permissions to user and group and nothing to others. 7 = 4+2+1 = rwx.
- `chmod u=rwx,g=rwx,o=r <f/d_name>` — same thing but in another format and we gave the "others" read permission.
- `chmod a+x <f/d_name>` — we gave the execution permission for all.
- `chmod -R 750 <PATH>` — the `-R` option will grant the given permission recursively.
- `chmod ug+rw <filename>` — we gave user and group owner the rw permission.

### umask

Let's make it simple and define `umask` in simple English. `umask` is used to set the file permissions for newly created files.

> Default file permissions: 666
>
> Default directory permissions: 777

- `umask 052` — as we've said above the file default permissions is 666, so if we run this command the file permissions for the newly created file will be 614 (666-052). Same thing for directories, the new permission will be 725 (777-052).

Using the `umask` command is temporary. To make it persistent, edit the umask in these files:

1. `vim ~/.bashrc`
2. `vim ~/.bash_profile` — to change umask for a specific user.
3. `vim /etc/profile` — to change umask for all users.
4. `vim /etc/login.defs` — as a root user.
5. Add `umask.sh` under `/etc/profile.d`.

### Special Permissions

The utility of special permissions is very simple.

| Permissions    | File               | Directory                         |
| :------------: | :----------------: | :-------------------------------: |
| SUID (4)       | Run as owner       | N/A                               |
| SGID (2)       | Run as group owner | Inherit directory group owner     |
| Sticky bit (1) | N/A                | Delete only if owner              |

Adding the Set-User-ID will permit you to run the file with the owner permission.

Now that we understand the special permissions let's learn how to use them.

- `chmod u+s <filename>` | `chmod 4750 <filename>` — adding SUID permission.
- `chmod g+s <f/d_name>` | `chmod 2750 <filename>` — adding SGID permission.
- `chmod +t <dirname>` | `chmod 1750 <dirname>` — adding the Sticky bit permission.

### ACLs

Access Control List is used to give a specific permission to a specific user or group or others.

Usage:

- `getfacl <f/d_name>` — to get the file access control list of the file or directory.
- `setfacl -R -m d:g:redhat:rw- </webapp/myapp>` — `-R` refers to recursively, `-m` refers to modify, `d` refers to default. This command will give the redhat group the rw permissions recursively and by default for `/webapp/myapp`.

### Attributes

File attributes are extra features you can use to tune a given file.

- `lsattr <filename>` — to list the attributes of the file.
- `chattr <filename>` — to change the attributes of the file.

Below is a summary of the most common attributes:

- `A` — When the file is accessed the atime is not updated. Good for minimizing disk I/O on a laptop.
- `a` — When this file is opened, it is opened in append only mode for writing.
- `c` — This file is automatically compressed on the disk by the kernel.
- `i` — This file cannot be modified, renamed or deleted.

### Practice

As a root user create a file with name `script` under directory `redhat`.
Make `redhat` the group owner of this directory and `chxmxii` the owner.
The owner of the `redhat` directory will have full permission while the group owner will have read and write only, others will have nothing. Also ensure that only the owner can delete the `script` file.
The group `students` will have the read and write permission on this file.
Change the attributes so the `script` file cannot be modified, renamed or deleted.

---

```bash
mkdir /redhat; touch /redhat/script
chown -R chxmxii:redhat /redhat/
chmod -R 1760 /redhat/
setfacl -R -m d:g:students:rw- /redhat/
chattr +i /redhat/script
```
