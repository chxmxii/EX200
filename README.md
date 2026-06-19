# RHCSA Study Notes

A structured collection of study notes covering the Red Hat Certified System Administrator (RHCSA) exam objectives. These chapters are organized by exam domain and provide command references, configuration procedures, and practice exercises.

## Chapters

### [Text Tools](content/01-text-tools.md)

- Use head and tail to display specific line ranges from files.
- Filter and transform text streams using sed substitution and deletion commands.
- Extract fields from structured files using cut with custom delimiters.
- Search file contents with grep using recursive, case-insensitive, and context options.
- Process and print fields from text files using awk with field separators.

### [Finding Files](content/02-finding-files.md)

- Locate binaries, source files, and manual pages using whereis.
- Search for files by name using locate after refreshing the database with updatedb.
- Use find to filter files by name, type, user, size, and permissions.
- Execute actions on matched files using the find -exec option.

### [Compressing Files](content/03-compressing-files.md)

- Create and extract tar archives using the -c and -x flags.
- Apply gzip, bzip2, or xz compression to tar archives with -z, -j, or -J options.
- List the contents of a compressed archive without extracting using tar -t.
- Compress and decompress individual files using gzip, gunzip, bzip2, and bunzip2.

### [Links](content/04-links.md)

- Create symbolic links with ln -s to reference files across filesystems.
- Create hard links with ln to share inode data between filenames.
- Identify hard-linked files by comparing inode numbers with ls -i.
- Understand that deleting a hard link does not remove data until all links are removed.

### [Ownership and Permissions](content/05-ownership-permissions.md)

- Change file ownership and group using chown and chgrp.
- Set numeric and symbolic permissions with chmod for user, group, and others.
- Configure default permissions for new files and directories using umask.
- Apply SUID, SGID, and sticky bit special permissions to files and directories.
- Manage fine-grained access using setfacl and getfacl for ACL entries.
- Protect files from modification or deletion using chattr immutable attribute.

### [Users and Groups](content/06-users-groups.md)

- Create, modify, and delete users with useradd, usermod, and userdel.
- Manage groups using groupadd, groupdel, and groupmod commands.
- Understand the fields in /etc/passwd and /etc/shadow configuration files.
- Configure password policies using /etc/login.defs and /etc/security/pwquality.conf.
- Populate new user home directories with default files from /etc/skel.

### [Storage Management](content/07-storage-management.md)

- Create disk partitions using fdisk for MBR and gdisk for GPT partition tables.
- Create and manage xfs and ext filesystems with mkfs, tune2fs, and xfs_admin.
- Mount filesystems persistently via /etc/fstab and verify with mount -a.
- Configure LVM by creating physical volumes, volume groups, and logical volumes.
- Set up Stratis pools and filesystems with automatic thin provisioning.
- Create VDO volumes for storage deduplication and compression.
- Encrypt block devices using LUKS with cryptsetup and /etc/crypttab.

### [Remote Filesystems](content/08-remote-filesystems.md)

- Configure an NFS server by editing /etc/exports and exporting shares.
- Mount NFS shares on clients persistently using /etc/fstab with _netdev option.
- Use showmount -e to discover available NFS exports from a server.
- Configure autofs for on-demand mounting using /etc/auto.master and map files.

### [Network Management](content/09-network-management.md)

- Manage network connections using nmcli to add, modify, and activate interfaces.
- Configure static IP addresses, gateways, and DNS servers with nmcli conn modify.
- Edit network interface configuration files in /etc/sysconfig/network-scripts/.
- Use nmtui as a text-based interface for network configuration.
- Enable IP forwarding by setting net.ipv4.ip_forward in /etc/sysctl.conf.
- Change the system hostname with hostnamectl set-hostname.

### [SSH and Remote Access](content/10-ssh-remote-access.md)

- Connect to remote systems securely using ssh with username and host address.
- Transfer files between hosts using scp for secure copy operations.
- Switch users with su and execute privileged commands with sudo.
- Configure sudo access by editing /etc/sudoers with visudo or using the wheel group.
- Manage the SSH daemon configuration through /etc/ssh/sshd_config.

### [Systemd](content/11-systemd.md)

- Manage service lifecycle with systemctl start, stop, enable, and disable commands.
- View unit status and configuration using systemctl status and systemctl cat.
- Edit unit files with systemctl edit --full and apply changes with daemon-reload.
- Identify boot performance issues using systemd-analyze blame.
- Understand systemd unit file locations in /usr/lib/systemd/system and /etc/systemd/system.

### [Processes](content/12-processes.md)

- View running processes with ps aux showing user, PID, CPU, and memory usage.
- Terminate processes using kill, pkill, and killall with appropriate signals.
- Manage foreground and background jobs using fg, bg, and the ampersand operator.
- Adjust process priority using nice to set and renice to change nice values.
- Monitor real-time system resource usage with the top command.

### [Scheduling Tasks](content/13-scheduling-tasks.md)

- Schedule recurring jobs by editing the cron table with crontab -e.
- Use anacron for periodic tasks that tolerate flexible execution timing.
- Schedule one-time jobs using the at command and manage them with atq and atrm.
- Store system-wide cron jobs in /etc/cron.d with proper crontab format.
- List and remove user cron entries with crontab -l and crontab -r.

### [Software Management](content/14-software-management.md)

- Install, update, and remove packages using yum with dependency resolution.
- Query installed packages and list file contents using rpm -q options.
- Configure local and remote repositories by creating .repo files in /etc/yum.repos.d/.
- Install package groups and module streams with yum group install and yum module.
- Review and undo package transactions using yum history.

### [System Logging](content/15-system-logging.md)

- Query the systemd journal using journalctl with unit, priority, and time filters.
- Enable persistent journal storage by setting Storage=persistent in journald.conf.
- Configure rsyslog rules to route messages by facility and priority to log files.
- Manage log file rotation and retention policies using logrotate.

### [Firewall](content/16-firewall.md)

- Manage firewall rules using firewall-cmd to add and remove services and ports.
- Assign network interfaces to zones for applying different trust levels.
- Apply permanent firewall changes with the --permanent flag followed by --reload.
- List available zones and services with --get-zones and --get-services options.

### [SELinux](content/17-selinux.md)

- Check and change SELinux enforcement mode using getenforce and setenforce.
- Set persistent SELinux mode by editing /etc/sysconfig/selinux.
- Manage file contexts with semanage fcontext and apply them using restorecon.
- Toggle SELinux booleans with setsebool -P for persistent policy adjustments.
- Troubleshoot SELinux denials by reviewing AVC messages in audit logs.
- Use sealert to interpret SELinux log events and obtain remediation guidance.

### [Web Server](content/18-web-server.md)

- Install the Apache HTTPD package and enable the service with systemctl.
- Configure the web server through /etc/httpd/conf/httpd.conf and conf.d/ directory.
- Create a basic test page under /var/www/html/ and verify with curl.

### [Kernel Management](content/19-kernel-management.md)

- Load and unload kernel modules using modprobe and view details with modinfo.
- Update and install kernels using yum and manage boot entries with grubby.
- Modify GRUB boot parameters by editing /etc/default/grub and regenerating grub.cfg.
- Switch between systemd targets using systemctl isolate and set-default.
- Reset the root password by appending rd.break to the kernel boot line.
- Tune kernel parameters at runtime using sysctl and persist changes in /etc/sysctl.conf.

### [Tuned Profiles](content/20-tuned-profiles.md)

- Optimize system performance by selecting pre-defined tuned profiles.
- List available profiles and check the active profile using tuned-adm.
- Apply a specific performance profile with tuned-adm profile command.
- Manage the tuned service using systemctl for start, stop, and status operations.
