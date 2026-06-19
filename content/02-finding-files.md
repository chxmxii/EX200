# Finding Files

There are only a few things to cover in this module.

## Commands

Ensure to run `updatedb` from time to time.

Starting with `whereis`:

- `whereis <command>` - used to locate the binary, source, and manual page files. Example: `whereis cd` outputs `cd: /usr/bin/cd /usr/share/man/man1/cd.1.gz /usr/share/man/man1p/cd.1p.gz`
- `locate <filename>` - used to find files by name
- `which <command>` - shows the full path of shell commands
- `find` - search for files in a directory hierarchy

`find` is the most used command. Use `man find` to learn more about this command.

### Practice

Find all the `.conf` files in the kernel by user `chxmxii` with size under 10KB having the SUID and SGID permission and copy them to a directory called `confiles`.

---

```bash
mkdir /confiles
find / -name "*.conf" -type f -user chxmxii -size -10k -perm 6000 -exec cp {} /confiles \; 2>/dev/null
```
