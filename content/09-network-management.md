# Network Management

## NetworkManager

As the name implies, NetworkManager is one of the utilities that is used to manage networks, including network interface cards (NIC) in Linux.

- `systemctl enable --now NetworkManager` - enable the NetworkManager service
- `vim /etc/sysconfig/network-scripts/ifcfg-<nicname>` - the network interface configuration file
- `ip a` - display the network cards available on a Linux system

**Note:** You can modify network properties using the network manager command-line interface `nmcli` or the network manager text user interface `nmtui`.

This is all you need to know about networking. Now let's learn how to modify network properties (IPv4, IPv6, DNS, gateway, prefix) using different options. Before doing this, make sure to restart your NetworkManager using the command below.

```bash
systemctl restart NetworkManager
# or
nmcli conn down <your_nic_name> && nmcli conn up <your_nic_name>
# or
nmcli conn reload
```

## Modifying the Configuration File

You can change the network properties using the configuration file below:

`/etc/sysconfig/network-scripts/ifcfg-<nicname>`

All you have to do is add the `IPADDR`, `NETMASK`, `DNS`, and `GATEWAY` to the file. Make sure to change the `BOOTPROTO` to `"none"`.

## Using nmcli

`nmcli` is a command-line client for NetworkManager. It allows controlling NetworkManager and reporting its status. For more information please refer to the `nmcli(1)` manual page.

We are going to use long commands with `nmcli` which are hard to memorize. `bash-completion` will be helpful and ready to compose them for us.

- `yum install -y bash-completion` - install the bash-completion package
- `rpm -qa | grep bash-completion` - verify the installation

Now here are a few examples using the `nmcli` command:

1. `nmcli conn show` - list the connections
2. Adding a new connection type ethernet:

```bash
nmcli conn add con-name network_name ifname ens160 ipv4.addresses 192.168.255.135/24 ipv4.gateway 192.168.0.1/24 ipv4.dns 8.8.8.8 type ethernet
```

3. Modifying the settings of the device ens160:

```bash
nmcli conn modify ens160 ipv4.addresses 192.168.132.125/24 ipv4.gateway 192.168.10.1 ipv4.dns 8.8.8.8 ipv4.method manual connection.autoconnect yes
```

**Note:** It is very important to set the method to manual, otherwise it will default to DHCP method. Remember, we did this too in the configuration file when we set `BOOTPROTO="none"`.

To add an IPv4 or IPv6 address with `nmcli` use the commands below:

- `nmcli conn modify ens160 +ipv4.addresses 192.168.22.10/24`
- `nmcli conn modify ens160 ipv6.method manual ipv6.addresses 3731:54:65fe:2::a8`

## nmtui

`nmtui` (network manager text user interface) will do the same job. It is also easier than all the other options and is beginner-friendly. However, learning the `nmcli` command is recommended.

## Additional Information

To change the hostname:

- `hostnamectl set-hostname <your_hostname>`
- `vim /etc/hostname`

`/etc/resolv.conf` is the name of a computer file used in various operating systems to configure the system's Domain Name System (DNS) resolver.

For more advanced information, consult the [Red Hat IP Command Cheatsheet](https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf).

## IP Forwarding

IP forwarding is a process used to determine which path a packet or datagram can be sent. The process uses routing information to make decisions and is designed to send a packet over multiple networks.

- `sysctl -a | grep net.ipv4.ip_forward` - check if IP forwarding is enabled
- `cat /proc/sys/net/ipv4/ip_forward` - check the current value

To enable IP forwarding (change the value to `0` to disable):

```bash
sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
```
