# Finding Files

There is only a few things to cover in this module.

## Commands :&#x20;

Ensure to <mark style="color:green;">updatedb</mark> time to time.\
Starting with the whereis :&#x20;

* <mark style="color:yellow;">whereis</mark> \<command> -> this command is used to locate the binary, source, and manual page files, <mark style="color:green;">example</mark> <mark style="color:red;">whereis</mark> cd <mark style="color:red;"><-></mark> <mark style="color:blue;">cd: /usr/bin/cd /usr/share/man/man1/cd.1.gz /usr/share/man/man1p/cd.1p.gz</mark>
* <mark style="color:yellow;">locate</mark> <mark style="color:blue;">\<filename></mark> -> used to find files by name
* <mark style="color:yellow;">which</mark> \<command> -> shows the full path of (shell) commands.
* <mark style="color:yellow;">find</mark> -> search for files in a directory hierarchy

{% hint style="success" %}
**\<find> is the most used command, use man find to learn more about this amazing command.**
{% endhint %}

### Practice Time :&#x20;

Find all the conf files in the kernel by user chxmxii with size under 10Ko having the SUID and SGID perm and copy them to a directory called confiles.

#### Answer

```
mkdir /confiles
find / *.conf -type f -user chxmxii -size -10K -perm 6000 -exec cp {} /confiles ;\ 2>/dev/null  
```

{% hint style="success" %}
Great job, you've finished learning about Find Files, now lets move to the next chapter Compressing files using tar.
{% endhint %}
