---
description: Learn how to manage software in RHEL 8.
---

# Managing Software

## Package Manager

A package manager is a program used to manage software in Linux systems. It can download, update, search, and delete packages as necessary.  
Every package comes with metadata, which the package manager uses to automate the download and update of dependencies.

There are different types of package managers depending on the Linux distribution, each working with its own package format:

- **RedHat and CentOS** → `yum` (Yellowdog Updater, Modified)  
- **Kali Linux, Ubuntu** → `apt` (Advanced Package Tool)  
- **Arch Linux** → `pacman`  
- **OpenSUSE** → `zypper`  

➡️ Software installation in Linux requires using the distribution’s package manager.

---

## RPM

- **RPM** stands for Red Hat Package Manager. It is used to install and query packages.  
- A package contains an archive of files compressed with `cpio`, metadata, and dependencies. It might also contain scripts to verify environment setup.  
- A **repo** is an online resource containing many packages.

### Common Commands

- `rpm -ivh <package>` → Install a package  
  Example: `rpm -ivh kernel-3.10.0-862.2.3.el7.x86_64.rpm`  

- `rpm -Uvh <package>` → Update a package  
  Example: `rpm -Uvh kernel-3.10-862.2.3.el7.x86_64.rpm`  
  (`i` = install, `U` = update)  

- `rpm -e <package>` → Remove a package  

- `rpm -qd <package>` → List a package’s documentation files  
- `rpm -ql <package>` → List all files in a package  
- `rpm -qc <package>` → List configuration files in a package  
- `rpm -qa | grep <package>` → Verify installed packages  
- `rpm -qp --scripts <package>` → View installation scripts of a non-installed package  
- `rpm -q --scripts <package>` → View installation scripts of an installed package  

---

## YUM

**YUM** (Yellowdog Updater, Modified) is an easier package manager tool that resolves dependencies (avoiding “dependency hell”) compared to `rpm`.

### Common Commands

- `yum install -y <package>`  
  Example: `yum install -y httpd`  

- `yum group install <group>`  
  Example: `yum group install "Smart Card Support"`  

- `yum info <package>`  
  Example: `yum info nginx`  

- `yum remove -y <package>` → Remove a package  

- `yum check-update kernel` OR `yum update` → Update packages  

- `yum provides <package>` → Search deeply for a package  

- `yum list --all` OR `yum search <package>` → List all packages  
- `yum list --installed` OR `yum list --available`  

- `yum config-manager --help`  

- `yum repolist` OR `yum repoinfo`  

- `yum history undo <id>` | `yum history redo <id>`  

---

### Small Lab

**Task:** Install the previous version of PHP (7.3).  

**Solution:**

```bash
yum module list
yum module profile php
yum module install php:7.3
```

## Repository

A **repository (repo)** is a central server where software packages are stored (similar to the Play Store).

---

### Setup a Local Repository

1. **Insert the disk and create an ISO**:

   `dd if=/dev/sr0 of=/rhel8.iso bs=1M`

2. **Create a mount point and update `/etc/fstab`**:

   `mkdir repo`  
   `echo "/rhel8.iso  /repo  iso9660 defaults 0 0" >> /etc/fstab`  
   `mount -a`

   > Sometimes you may need to mount `/dev/sr0` directly.

3. **Create `.repo` files** (two methods):

   - **Using `dnf`**:  
     `dnf config-manager --add-repo <repo_url>`

   - **Manually with `vim`**:

     `/etc/yum.repos.d/BaseOS.repo`:  
     ```
     [BaseOS]
     name=BaseOS
     baseurl=file:///repo/BaseOS
     gpgcheck=0
     enabled=1
     ```

     `/etc/yum.repos.d/AppStream.repo`:  
     ```
     [AppStream]
     name=AppStream
     baseurl=file:///repo/AppStream
     gpgcheck=0
     enabled=1
     ```

4. **Verify repositories**:  
   `yum repolist`

---

### Explanation of `.repo` Fields

- `[BaseOS]` → Unique repository ID  
- `name=` → Name of the repository  
- `baseurl=` → Repository URL (location of packages)  
  - Local: `file:///path-to-repo`  
  - HTTP: `http://url-repo`  
  - HTTPS: `https://url-repo`  
  - FTP: `ftp://path-to-repo`  
- `enabled=` → Enable/disable a repo (`1` = enable, `0` = disable)  
- `gpgcheck=` → Enable/disable GPG check (`1` = enabled, `0` = disabled)  
  - For local repos, set it to `0` to avoid failures.  

---

### Troubleshooting Yum Metadata Errors

If you encounter a metadata error with `yum`, set the subscription-manager release and clean your repolist.  
👉 More info: Red Hat Solution
