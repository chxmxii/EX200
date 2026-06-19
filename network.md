# Network Management

##

## NetworkManager :&#x20;

As the name implies, NetworkManager is one of the utilities that is used to manage network, including network interface cards (NIC) in Linux.

* <mark style="color:red;">systemctl</mark> enable --now <mark style="color:green;">NetworkManager</mark> -> to enable the NetManager service.
* <mark style="color:red;">vim</mark> /etc/sysconfig/network-scripts/<mark style="color:blue;">ifcfg-\<nicname></mark> ->  the network interface configuration file. ![](/files/QC6hJ3RHvhuAUgK1Ja01)
* <mark style="color:red;">ip</mark> a -> To see the network cards available on a Linux system, use the command.![](/files/do5aAXBv6k9mrX7fc11o)

{% hint style="info" %}
You can modify a network properties using **network-manage-command-line-interface** <mark style="color:red;">nmcli</mark> or **network-manager-text-user-interfac**e <mark style="color:blue;">nmtui</mark>.
{% endhint %}

This all you need to know about networking, now lets learn how to modify a network properties (ipv4-ipv6-dns-gateway-prefix-) using different options. before doing this make sure to restart your network manager using the command below.

```
systemctl restart NetworkManager
              or
nmcli conn down <your_nic_name> && nmcli conn up <your_nic_name>
              or
nmcli conn reload        
```

## Modifying the configuration file :&#x20;

You can change the network properties using the configuration file below.\
-> /etc/sysconfig/network-scripts/<mark style="color:blue;">ifcfg-\<nicname></mark>

All you have to do is adding the IPADDR-NETMASK-DNS-GATEWAY on the file. \
Make sure to change the **BOOTPROTO** to **"none"**.

![](/files/i2gxvW7ZWGeFp0IJusFB)

## USING NMCLI :&#x20;

<mark style="color:blue;">**nmcli**</mark> is a command-line client for NetworkManager. It allows controlling NetworkManager and reporting its status. For more information please refer to nmcli(1) manual page.\
We are going to use long commands with <mark style="color:yellow;">**`nmcli`**</mark> which are hard to memorize. <mark style="color:blue;">**`bash-completion`**</mark> will be helpful & ready to compose them for us.\
&#x20;  \- <mark style="color:red;">yum install -y bash-completion</mark> -> to install the bash-completion package.\
&#x20;  \- <mark style="color:green;">rpm -qa | grep bash-completion</mark> -> to verify.

Now lemme show you an few examples using the nmcli command.<br>

1. <mark style="color:red;">nmcli conn show</mark> -> to list the connections.
2. <mark style="color:red;">nmcli</mark> <mark style="color:red;">conn</mark> <mark style="color:red;">add con-name</mark> network\_name <mark style="color:red;">ifname</mark> ens160 <mark style="color:red;">ipv4.addresses</mark> 192.168.255.135<mark style="color:blue;">/24</mark> <mark style="color:red;">ipv4.gateway</mark> 192.168.0.1/24 <mark style="color:red;">ipv4.dns</mark> 8.8.8.8 <mark style="color:red;">type</mark> ethernet \
   \=> Adding new connection type ethernet.
3. <mark style="color:red;">nmcli</mark> <mark style="color:red;">conn modify</mark> ens160 <mark style="color:red;">ipv4.adresses</mark> 192.168.132.125/<mark style="color:blue;">24</mark> <mark style="color:red;">ipv4.gateway</mark> 192.168.10.1 <mark style="color:red;">ipv4.dns</mark> 8.8.8.8 <mark style="color:red;">ipv4.method</mark> manual <mark style="color:red;">connection.autoconnect</mark> yes\
   \=> Modifying the setting of the device ens160.&#x20;

{% hint style="danger" %}
Is very important to set the <mark style="color:yellow;">**method to manual**</mark>, otherwise it will set to <mark style="color:blue;">DHCP</mark> method by default. remember? we did this too in this configuration file when we did **BOOTPROTO="none"**
{% endhint %}

To add an ipv4 or ipv6 address with nmcli use the command below:

* <mark style="color:red;">nmcli conn modify</mark> ens160 **+**<mark style="color:red;">ipv4.adresses</mark> 192.168.22.10/24
* <mark style="color:red;">nmcli conn modify</mark> ens160 <mark style="color:red;">ipv6.method</mark> manual <mark style="color:red;">ipv6.adresses</mark> 3731:54:65fe:2::a8

## nmtui :&#x20;

nmtui - network manager text user interface will do the same job, also is more easier than all the options and is "noob-friendly".  I will let the pictures speak haha.

![](/files/lqLTvabQ2NB8UYqBSV3V)![](/files/q88lJRKmVi03JapFYxCq)![](/files/AlvPnrAsJjdQc54PImkL)![](/files/Ne3wwz1wFSClw02ZE7fX)![](/files/kHgQVNTHQgMjWVpjyidS)

See, ain't this easy? but honestly, I would recommend learning the nmcli command instead.

## Additional informations :&#x20;

to change the hostname :&#x20;

* <mark style="color:red;">hostnamectl set-hostname</mark> \<your\_hostname>
* vim <mark style="color:red;"><mark style="color:green;">/etc/hostname<mark style="color:green;"></mark>&#x20;

<mark style="color:blue;">**`/etc/resolv.conf`**</mark> is the name of a computer file used in various operating systems to configure the system's Domain Name System (DNS) resolver.

consult this document for more advanced informations : <br>

{% embed url="<https://docs.google.com/viewerng/viewer?url=https%3A%2F%2Faccess.redhat.com%2Fsites%2Fdefault%2Ffiles%2Fattachments%2Frh_ip_command_cheatsheet_1214_jcs_print.pdf>" %}
Redhat IP command cheatsheet.
{% endembed %}

### IP Forwarding :&#x20;

IP forwarding is a process used to determine which path a packet or datagram can be sent. The process uses routing information to make decisions and is designed to send a packet over multiple networks.

* <mark style="color:red;">sysctl</mark> -a | <mark style="color:red;">grep</mark> net.ipv4.ip\_forward&#x20;
* <mark style="color:blue;">cat</mark> /proc/sysnet/ipv4/ip\_forward\
  \=> to check if **IP Forwarding** is <mark style="color:green;">enabled</mark>.
* sysctl -w net.ipv4.ip\_forward = 1 \
  echo " net.ipv4.ip\_forward= 1"  >> /etc/sysctl.conf\
  \=> to enable the **IP** **Forwarding**, change the value to <mark style="color:red;">0</mark> if you wish to disable.&#x20;

{% hint style="success" %}
Can't believe it, we made it haha! now we are free to move to the next chapter. great job body! don't forget to practice.
{% endhint %}
