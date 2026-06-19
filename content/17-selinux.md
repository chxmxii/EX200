# SELinux

SELinux means security-enhanced Linux. It is an advanced security feature/utility in Linux.

SELinux cannot be replaced with antivirus, firewall, permissions, and ACLs on Linux. As the name implies, it is a security-enhanced feature and not a replacement of other security features.

- `ps auxZ` or `ls -lZ` — show SELinux context labels
- `sestatus -v` — check the current SELinux status
- `getenforce` — same thing
- `setenforce 0` — set SELinux in permissive mode
- `setenforce 1` — set SELinux to enforcing mode

The last two commands change SELinux temporarily, not permanently. If you want to make the change permanent, you have to edit the SELinux configuration file located at `/etc/sysconfig/selinux`.

- **Enforcing Mode:** SELinux is fully operational and enforcing all SELinux rules in the policy.
- **Permissive Mode:** All SELinux-related activity is logged, but no access is blocked.
- **Disabled Mode:** Never set to disabled, unless you know that is what you really want.

## SELinux Contexts

The SELinux policy uses these contexts in a series of rules which define how processes can interact with each other and the various system resources. By default, the policy does not allow any interaction unless a rule explicitly grants access.

SELinux contexts have several fields: `user`, `role`, `type`, and `security level`. The SELinux type information is the most important when it comes to the SELinux policy, as the most common policy rule which defines the allowed interactions between processes and system resources uses SELinux types and not the full SELinux context.

```
user_context:role_context:type
```

For example, the SELinux type name for the web server process is `httpd_t`. The type context for files and directories normally found in `/var/www/html/` is `httpd_sys_content_t`.

### How Context Settings Are Applied

- If a new file is created, it inherits the context settings from the parent directory.
- If a file is copied to a directory, this is considered a new file, so it inherits the context settings from the parent directory.
- If a file is moved, or copied while keeping its properties (by using `cp -a`), the original context settings of the file are applied.

**Note:** When using `mv` command make sure to include the `-Z` option, so it will inherit the context of the new directory.

### Managing File Contexts

Use the general purpose `semanage` command to define file, port, and other object contexts.

- `semanage fcontext` writes a file context into the SELinux policy for use.

For file system based objects, tweaking a policy does not take effect immediately.

- Use `restorecon` to enforce a policy on the file system:

```bash
semanage fcontext -a -t system_u:object_r:etc_t "/etc(/.*)?"
restorecon -Rv /etc
```

- Another option is to `touch /.autorelabel` and reboot.
- To see the context of a port use:

```bash
netstat -Ztulpen
```

## Booleans

Booleans are a higher level concept for turning on/off a complete set of functionality.

- `getsebool -a` — list all booleans
- `setsebool -P httpd_enable_homedirs on` — toggle a boolean

Example: allow httpd to see home directories for a public webpage.

Show all booleans and pipe to grep for httpd:

```bash
getsebool -a | grep httpd
httpd_enable_homedirs --> off
```

Change boolean:

```bash
setsebool -P httpd_enable_homedirs on
```

## Logging

- Default uses `auditd`, logs are not human friendly: `grep AVC /var/log/audit/audit.log`
- AVC = access vector cache, and is a signature of SELinux logs
- `sealert` parses raw audit log events, adds value, and writes to `/var/log/messages`
- Run `sealert <uuid>` to get advice on a known event
- Use `journalctl | grep sealert` to locate UUID

### Troubleshooting with SELinux

If a service is not working, always suspect SELinux:

1. Check if it is running: `getenforce`
2. Temporarily relax to permissive mode: `setenforce 0`
3. Re-test. If the service is operational, you know SELinux is to blame.
4. Check logs: `grep sealert /var/log/messages`
