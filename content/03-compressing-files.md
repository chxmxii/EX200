# Compressing Files

`tar` is the most used tool to compress files in different formats. Consult the man page for more information and options.

**Note:** Use `ls -l --block-size=MB` to verify gain after compressing.

## Tar

To compress all the files under `/etc` and name it `etc-archive`, use this command:

```bash
tar -cvf etc-archive.tar /etc
```

- `-c` is for compress
- `-v` is for verbose
- `-f` for file

To extract a compressed file with name `project.tar`, use this command:

```bash
tar -xvf project.tar
```

- `-x` is for extract
- `-v` is for verbose
- `-f` for file

Here is a summary of the most common options:

- `-t` — list the content of the compressed file
- `-x` — extract a compressed file
- `-C` — specify a path
- `-z` — GZIP format
- `-j` — BZIP2 compression
- `-J` — XZ compression

## Other Tools

- `gzip /root/etc.tar.gz /etc` — to compress the contents of `/etc` under `/root`
- `bzip2 /root/home.tar.bz2 /home` — to compress the contents of `/home` under `/root`
- `gunzip /root/etc.tar.gz` — to decompress contents of `/root/etc.tar.gz`
- `bunzip2 /root/home.tar.bz2` — to decompress contents of `/root/home.tar.bz2`
- Use the `-k` option to keep the original file

**Note:** Consult the man page for more information.

### Practice

Using the `tar` tool and bzip2 format, compress all the files under `/tmp` and ensure the archived file is under `/root/compressedfile`. Then decompress it under `/` and make sure to keep the archived file.

---

```bash
tar -cjvf /root/compressedfile/tmp.tar.bz2 /tmp
tar -xvf /root/compressedfile/tmp.tar.bz2 -C /
```
