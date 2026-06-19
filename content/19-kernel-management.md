# Kernel Management

The Linux kernel is the heart of the operating system. It is the layer between the user, who works with Linux from a shell environment, and the hardware on the computer.

## Basic Kernel Management

- `modprobe <module-name>` — manually load a kernel module
- `modprobe -r <module-name>` — manually unload a kernel module
- `modinfo <module-name>` — list parameters supported by a module
- Module parameters can be edited under `/etc/modprobe.d/`
- `/proc` directory provides a view of the kernel internals
- `sysctl -a` — dump all kernel tunables

## Updating and Installing Kernel

Updating and installing kernels is a common task in RHCSA. Here is how:

- `uname -r` — get current kernel version
- `yum list kernel --showduplicates` — view available versions
- `yum update kernel` — updates the kernel (creates a new `vmlinuz` file)
- `rpm -ql kernel | grep "/boot/vmlinuz"` — verify installation
- If `vmlinuz` is missing, run `dracut`
- `yum list kernel` — confirm installation

### Installing a New Kernel

```bash
yum install kernel
grubby --info=ALL        # shows all installed kernels; index 0 is the default
grubby --set-default-index 0   # set default kernel by index
```

Alternatively:

```bash
grub2-set-default 0    # most recent kernel
grub2-set-default 1    # second most recent kernel
```

Reboot to verify.

### Setting a Previous Kernel as Default

```bash
uname -r                          # check current default kernel
rpm -qa kernel                    # list installed kernels
grubby --info=ALL                 # see all kernels
grubby --default-index            # check default index
cp /boot/grub2/grub.cfg /boot/grub2/grub.cfg_backup   # create backup
grubby --set-default-index=1      # set previous kernel as default
grubby --default-index            # confirm
grub2-mkconfig -o /boot/grub2/grub.cfg
```

Reboot to confirm.

## Modifying GRUB

Edit `/etc/default/grub` and remove `rhgb` and `quiet` to show boot messages.

Add to `GRUB_CMDLINE_LINUX`:

- `biosdevname=0`
- `net.ifnames=0` — use `eth*` naming for network interfaces

Apply changes:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
```

**Never edit `grub.cfg` directly** — changes will be lost after kernel updates.

Check if the system uses EFI:

```bash
dmesg | egrep -i "efi|bios"
mount | grep "^/"
```

If `/sys/firmware/efi` exists, the system is using EFI.

## systemd Targets

A target is a group of unit files. Isolatable targets define a desired final state:

- `emergency.target`
- `rescue.target`
- `multi-user.target`
- `graphical.target`

Services are linked into targets via their `WantedBy` setting (e.g., `systemctl cat httpd` shows `WantedBy=multi-user.target` in the `[Install]` section).

Default targets and their wants are defined in `/etc/systemd/system/` as symlinks.

To boot into a specific target:

- At GRUB2 prompt: append `systemd.unit=rescue.target` to the kernel line
- On a running system: `systemctl isolate <target>`

Useful commands:

- `systemctl list-dependencies` — view target/unit hierarchy
- `systemctl get-default` — check default target
- `systemctl set-default <target>` — set default target

Example:

```bash
systemctl set-default multi-user.target
```

## Essential Troubleshooting (Changing Root Password)

Edit the GRUB2 entry while booting and add `rd.break` to the end of the kernel line.

Once in the shell:

```bash
mount -o remount,rw /sysroot
chroot /sysroot
passwd
touch /.autorelabel
```

Press `Ctrl+D` twice to exit and reboot. The `touch /.autorelabel` command flags SELinux to relabel the filesystem on next boot.
