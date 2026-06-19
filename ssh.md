# SSH and Secure Copy

## SSH :&#x20;

SSH or Secure shell, is a known protocol used for connecting to server remotely, in RHCSAv8 you just have to learn how to connect from server to server.\
below you will find the command syntax :&#x20;

* <mark style="color:red;">systemctl</mark> enable --now <mark style="color:green;">sshd</mark> -> to enable the sshd service.
* <mark style="color:red;">ssh</mark> <<mark style="color:blue;">user\_name</mark>>@<<mark style="color:green;">ip</mark>*<mark style="color:green;">address/host\_</mark>*<mark style="color:green;">name</mark>>&#x20;
* vim <mark style="color:red;">/etc/ssh/sshd\_config</mark> -> configuration file for ssh.

Host key is stored in \~/.ssh/known\_hosts

Sensitive data will be sent through an encrypted connection.

## SCP :&#x20;

SCP (secure copy) is a command-line utility that allows you to securely copy files and directories between two locations. \
Below you will find the right command to do that :

* <mark style="color:red;">**scp**</mark> \<file\_name>  usernam&#x65;*<mark style="color:green;">@</mark>\<ip\_adress>*<mark style="color:red;">:</mark>/<mark style="color:blue;">remote</mark>/<mark style="color:blue;">directory</mark>

## chvt :

chvt - change foreground virtual terminal, The command <mark style="color:yellow;">**chvt**</mark> *<mark style="color:green;">N</mark>* makes *<mark style="color:yellow;">/dev/tty</mark><mark style="color:green;">N</mark>* the foreground terminal.&#x20;

* <mark style="color:red;">chvt</mark> 4 <mark style="color:red;">-></mark> change the virtual terminal to tty4.

or you can change the virtual terminal directly through your keyboard by clicking on&#x20;

> **ctrl + alt + F(1-2-3-4-5-6)**

## su :&#x20;

su - switch user, another powerful command used to switch from user to user.

* <mark style="color:green;">su</mark> <mark style="color:green;">-</mark> chxmxii <mark style="color:red;">-></mark> to switch from the current user to chxmxii
* <mark style="color:green;">su -</mark> <mark style="color:red;">-></mark> to switch from the current user to the root user.

{% hint style="info" %}
the "-" will take you to the home directory of the user. you are free to use it or not.
{% endhint %}

## sudo :&#x20;

sudo - super user do, use this command to execute tasks with root power.

* <mark style="color:green;">sudo</mark> userdel <mark style="color:green;">user1</mark> -> normal user cannot run this command without using sudo as only root can add and delete users.

to allow users running commands as root you need either to :&#x20;

* vim /<mark style="color:blue;">**etc**</mark>/<mark style="color:blue;">**sudoers**</mark> or <mark style="color:blue;">**visudo**</mark> and add the user \<username> under #allow root to run any commands anywhere
* ![](/files/wxbf9QdbznrNuPv6TrJf)-> I allowed user chxmxii to run any commands anywhere.
* &#x20;<mark style="color:red;">usermod</mark> -aG wheel <mark style="color:red;">\<username></mark> -> adding the user to the wheel group is alot easier. don't forget to uncomment this line. ![](/files/EYwWc8giLFlygeK3hd5s)

{% hint style="success" %}
phew, we are done now lets move to the next chapter.
{% endhint %}
