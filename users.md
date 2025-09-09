---
description: Learn how to manage users and groups
---

# Managing Users and Groups

## I. Users

Most common commands and files used to manage users.  
Consult `man <command>` for more information.

### Commands

- `useradd <username>` → add a new user.
- `userdel <username>` → delete an existing user.
- `usermod -option <username>` → modify a user’s characteristics.

### Files

- `/etc/default/useradd` → useradd default config file.
- `/etc/passwd` | `vipw` → verify the creation of your user with the wanted properties.
- `/etc/shadow` → stores encrypted user passwords and is accessible only to the root user.

---

## II. Groups

Most common commands and files used to manage groups.  
Consult `man <command>` for more information.

### Commands

- `groupadd <groupname>` → create a new group.
- `groupdel <groupname>` → delete an existing group.
- `groupmod <groupname>` → modify a group definition on the system.
- `lid -g <groupname>` → display the members of a group.
- `groupmems -g <groupname> -l` → administer members of a user’s primary group.
- Other commands: `groups`, `id`, `getent group` ...

### Files

- `/etc/group` | `vigr` → list groups in your system.
- `/etc/gshadow` → readable only by root, contains an encrypted password for each group.

---

## III. Configuration Files

### `/etc/passwd`

Example:

```
chxmxii:x:1000:1000:RHEL8VM:/home/chxmxii:/bin/bash
```

This line contains 7 fields separated by `:`:

1. **chxmxii** → username  
2. **x** → password placeholder  
3. **1000** → User ID  
4. **1000** → Group ID  
5. **RHEL8VM** → Comment (GECOS)  
6. **/home/chxmxii** → Home directory  
7. **/bin/bash** → Shell  

---

### `/etc/shadow`

Example:

```chxmxii:$1$KfyGjDYU$OPkmq6g6pFehQk0DxUiZ80:18988:0:99999:7:::```


This line contains 9 fields:

1. **chxmxii** → username  
2. **$1$KfyGjDYU$OPkmq6g6pFehQk0DxUiZ80** → encrypted password in the format *type$salt$hashed*  
3. **18988** → last password change (days since Jan 1, 1970)  
4. **0** → minimum number of days before the password can be changed  
5. **99999** → maximum number of days before the password must be changed  
6. **7** → warning period  
7. **?** → inactivity period (since Jan 1, 1970)  
8. **?** → expiration date (when the account is disabled)  
9. **?** → reserved for future use  

**Note:**  
- `!` in the password field means the password is locked.  
- `!!` means the password is disabled.  

---

## IV. Hints

- Edit password quality: `/etc/security/pwquality.conf`  
- Edit user configuration: `/etc/default/useradd`  
- Edit umask and password aging: `/etc/login.defs`  
- Always back up files before editing, e.g.:  

  ```bash
  cp /etc/login.defs /etc/login.defs-original

Files in /etc/skel will be copied to /home/$USER upon creation.

---

#### V. Practice Time

Task: Create user chxmxii with the following properties:
UID: 1544
GID: 1552
GECOS: RHELV8
Home Directory: /home/chxmxii
Shell: /bin/zsh
Add chxmxii to the wheel group
Ensure a file named Rules is created in the user’s home directory

SOL:

```shell
useradd -u 1544 -g 1552 -c "RHELV8" -d /home/chxmxii -s /bin/zsh chxmxii
usermod -aG wheel chxmxii
touch /etc/skel/Rules
```
