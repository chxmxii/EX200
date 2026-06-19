# Common text tools and grepping

## head & tail :&#x20;

head and tail are a great commands to show a specified number of lines, for example if you want to display the last <mark style="color:red;">5 line</mark>s from the file <mark style="color:blue;">/etc/passwd</mark> use the command syntax below :&#x20;

* <mark style="color:red;">tail</mark> -5 <mark style="color:blue;">/etc/passwd</mark>&#x20;

Head will do the same job but instead, it will display the first <mark style="color:red;">5 line</mark>s from the file <mark style="color:blue;">/etc/shadow</mark>

* <mark style="color:red;">head</mark> -5 <mark style="color:blue;">/etc/shadow</mark>

## CAT :&#x20;

Another nice command to display the content of a file. this command is more useful when you used **less | more**

* <mark style="color:red;">cat</mark> /etc/passwd | <mark style="color:green;">more</mark> -> to display the content of /etc/passwd
* <mark style="color:red;">tac</mark> /etc/passwd | <mark style="color:green;">less</mark> -> same thing but this time from tail to head.

## cut | sort | tr :&#x20;

these 3 commands makes a great utility. the cut command is used to cut a lines using a delimiter, followed by sort which is used to sort a file in a precise format. while tr is used for translation. lets learn how to use these commands. I will display the first field from /etc/passwd in upper cases and sort it reversely.

```
cut -f1 -d: /etc/passwd | sort -r | tr [:lower:] [:upper:]
```

the output of this command :

&#x20;![](/files/acKYo8GqCVXdwFIzigGo)

Below you will find a summary for the most common options for cut and sort :&#x20;

* Starting with <mark style="color:red;">**cut**</mark> :&#x20;

  &#x20;**`-b`** : **bytes**\
  &#x20;**`-c`** : **character**\
  &#x20;**`-f`** : **field** \
  &#x20;**`-d`** : **delimiter**
* Moving to the <mark style="color:red;">**sort**</mark> command : \
  &#x20;**`-r`** : **reverse**\
  &#x20;**`-d`** : **dictionary** order\
  &#x20;**`-n`** : **numerical** value\
  &#x20;**`-R`** : **Random** \
  &#x20;**`-b`** : **ignore** blanks\
  &#x20;**`-o`** : specify **output file**

{% hint style="info" %}
Another great command you can use with sort, is <mark style="color:red;">**unique**</mark>&#x20;

consult man <mark style="color:red;">**unique**</mark> for more informations.
{% endhint %}

## **sed and awk :**&#x20;

sed and awk are both powerful commands. sed (stream editor) used for filtering and transforming text. let me explain using some examples.

* <mark style="color:red;">sed</mark> -i 's/<mark style="color:green;">old</mark>*<mark style="color:green;">text</mark>/*<mark style="color:blue;">new\_text</mark>/*g' <mark style="color:red;">\<filename></mark> -> this command will change the old\_text to new text in **\<filename>** , the <mark style="color:blue;">'-i'</mark> option is used to ensure the changes in the **\<filename>**, <mark style="color:green;">s</mark> refers to substitute while <mark style="color:red;">g</mark> refers to global.*
* <mark style="color:red;">sed</mark> -i <mark style="color:blue;">'1d'</mark> <mark style="color:red;">\<filename></mark> -> deleting the first line from <mark style="color:blue;">\<filename></mark>

awk is a very advanced command,  is a pattern scanning and processing language.\
let me make it simple for you by using this command :&#x20;

* <mark style="color:red;">gawk</mark> -F: '{ print $1 }' <mark style="color:red;">/etc/passwd</mark> -> print the first field from /etc/passwd.\
  the <mark style="color:green;">"-F"</mark> option refers to field.

## Grep :&#x20;

grep is the most common command in unix, it is used to filtering while looking deeply in a file text. below you will find a summary for the most commin options for grep.

* &#x20;**`-R`** : **recursive**\
  &#x20;**`-v`** :  not containing "text"\
  &#x20;**`-i`** : **ignore case**\
  &#x20;**`-f`** : specify **file**\
  &#x20;**`-l`** : list the containing **files names**\
  &#x20;**`-L`** : list the **files names without match**\
  &#x20;**`-A <n>`** : print n lines **after**\
  &#x20;**`-B <n>`** : print n lines **before**\
  &#x20;**`-C <n>`** : print n lines **before** & **after**

{% hint style="success" %}
Am not going to cover all the text tools here, so make sure to consult redhat.com for more informations. lets move to the next chapter.
{% endhint %}
