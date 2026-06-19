# SSH and Remote Access

## SSH

SSH (Secure Shell) is a protocol used for connecting to servers remotely. In RHCSAv8 you need to learn how to connect from server to server.

Below you will find the command syntax:

```bash
systemctl enable --now sshd
```

- `ssh <username>@<ip_address/hostname>` — connect to a remote host
- `vim /etc/ssh/sshd_config` — configuration file for SSH

The host key is stored in `~/.ssh/known_hosts`.

Sensitive data will be sent through an encrypted connection.

## SCP

SCP (Secure Copy) is a command-line utility that allows you to securely copy files and directories between two locations.

Below you will find the right command to do that:

```bash
scp <file_name> username@<ip_address>:/remote/directory
```

## chvt

`chvt` (change foreground virtual terminal) makes `/dev/ttyN` the foreground terminal.

```bash
chvt 4
```

This changes the virtual terminal to `tty4`.

You can also change the virtual terminal directly through your keyboard by pressing:

> **Ctrl + Alt + F(1-6)**

## su

`su` (switch user) is a command used to switch from one user to another.

```bash
su - chxmxii
```

This switches from the current user to `chxmxii`.

```bash
su -
```

This switches from the current user to the root user.

Note: The `-` option takes you to the home directory of the target user. You are free to use it or not.

## sudo

`sudo` (super user do) is used to execute tasks with root privileges.

```bash
sudo userdel user1
```

A normal user cannot run this command without using `sudo`, as only root can add and delete users.

To allow users to run commands as root you need either to:

- Edit `/etc/sudoers` using `visudo` and add the user under "Allow root to run any commands anywhere"
- Run `usermod -aG wheel <username>` to add the user to the `wheel` group (do not forget to uncomment the relevant line in the sudoers file)
