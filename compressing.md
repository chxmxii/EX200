# Compressing Files

Tar is the most used tool to compress file in different formats, Consult man for more informations and options.

{% hint style="info" %}
Use <mark style="color:red;">**ls -l --block-size:MB**</mark> to verify gain after compressing
{% endhint %}

### Tar :&#x20;

to compress all the files under /etc and name it etc-archive, use this command:

#### <mark style="color:red;">**tar**</mark>**&#x20;-cvf&#x20;**<mark style="color:blue;">**etc-archive.tar**</mark>**&#x20;**<mark style="color:yellow;">**/etc**</mark>&#x20;

* <mark style="color:green;">-c</mark> is for compress&#x20;
* <mark style="color:green;">-v</mark> is for verbose&#x20;
* <mark style="color:green;">-f</mark> for file.&#x20;

#### to extract a compressed file with name project.tar, use this command :&#x20;

<mark style="color:red;">**tar**</mark>**&#x20;-xvf&#x20;**<mark style="color:yellow;">**project.tar**</mark>&#x20;

* <mark style="color:green;">-x</mark> is for extract.
* <mark style="color:green;">-v</mark> is for verbose.
* <mark style="color:green;">-f</mark> for file.

Here is a summary of the most common options:&#x20;

* <mark style="color:blue;">-t</mark> -> cat the content of the compressed file.
* <mark style="color:blue;">-x</mark> -> extract a compressed file.
* <mark style="color:blue;">-C</mark> -> specify a path.
* <mark style="color:blue;">-z</mark> -> GZIP format.
* <mark style="color:blue;">-j</mark> -> BZIP2 compression .
* <mark style="color:blue;">-J</mark> -> XZ compression.&#x20;

### Other tools :&#x20;

* <mark style="color:red;">**gzip**</mark> /root/etc.tar.gz **/etc** <mark style="color:blue;">-></mark> to compress the contents of **/etc** under **/root**.
* <mark style="color:red;">**bzip2**</mark> /root/home.tar.bz2 /home <mark style="color:blue;">-></mark> to compress the contents of **/home** under **/root**.
* <mark style="color:red;">**gunzip**</mark> /root/etc.tar.gz <mark style="color:blue;">-></mark>  to decompress contents of <mark style="color:blue;">**/root/etc.tar.gz**</mark>
* <mark style="color:red;">**bunzip2**</mark>  /root/home.tar.bz2 <mark style="color:blue;">-></mark> to decompress contents of <mark style="color:blue;">**/root/home.tar.bz2**</mark>
* use the <mark style="color:green;">-k</mark> option to keep the original file.&#x20;

{% hint style="info" %}
consult the man page for more informations.&#x20;
{% endhint %}

### Practice Time :&#x20;

Using the tar tool and bzip2 format, compress all the files under /tmp and ensure the archived file is under /root/compressedfile. then decompress it under / and make sure to keep the archived file.

#### Answer :&#x20;

```
tar -cjvf /root/compressedfile/tmp.tar.bz2 /tmp
tar -xvf /root/compressedfile/tmp.tar.bz2 -C /
```

{% hint style="info" %}
Now, lets move to the next chapter links.
{% endhint %}
