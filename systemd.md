# Working With Sytemd

## Systemd :&#x20;

Master of everything and the first process/service ( <mark style="color:red;">PID = 1</mark> ) to start after the Linux kernel boot. Hence, our guy <mark style="color:blue;">**systemd**</mark> will starts up and supervises almost the entire system units : -> Services\
&#x20;                       -> Mounts\
&#x20;                       -> Sockets\
&#x20;                       ->Targets\
use <mark style="color:blue;">**systemctl -t help**</mark> to list all units. \
-) <mark style="color:yellow;">systemd-analyze blame</mark> -> in case you want to know what takes time on boot.\
-) <mark style="color:yellow;">systemd-analyze security</mark> -> check the security of a unit.

### Systemd Files : &#x20;

-) <mark style="color:green;">/usr/lib/systemd/system -></mark> default unit files that have been installed from RPM    \
&#x20;-) <mark style="color:green;">/etc/systemd/system -></mark> All costume unit files\
&#x20;-) <mark style="color:green;">/run/systemd/system -></mark> All units files that have automatically been generated in the run-time.

{% hint style="danger" %}
Prevent modifying a unit file in <mark style="color:yellow;">/usr/lib/systemd/system</mark> unless you know what you are doing.
{% endhint %}

#### Systemd Status :&#x20;

<mark style="color:blue;">A unit $status can be :</mark> \
Loaded - <mark style="color:green;">Active</mark> - Running - Exited - Waiting - <mark style="color:red;">Dead</mark> - <mark style="color:green;">Enabled</mark> - <mark style="color:red;">Disabled</mark> - Static.

## Systemctl :&#x20;

Used command to manage all the units through systemd.\
Below, you will find almost all the commands to help you managing units.

* <mark style="color:green;">systemctl</mark> --type=services --all <mark style="color:blue;">-></mark> list all the services (active, inactive)
* <mark style="color:green;">systemctl</mark> list-units <mark style="color:blue;">-></mark> display all the active units that systemd know about.
* <mark style="color:green;">systemctl</mark> --failed --type=services
* <mark style="color:green;">systemctl</mark> status -l \<service\_name>
* <mark style="color:green;">systemctl</mark> cat \<service\_name> <mark style="color:blue;">-></mark> current config of this unit.
* <mark style="color:green;">systemctl</mark> show \<service\_name> <mark style="color:blue;">-></mark> show available configs
* <mark style="color:green;">systemctl</mark> edit --full \<service\_name> <mark style="color:blue;">-></mark> to modify the default configs. \
  watch this video : <https://www.youtube.com/watch?v=xXGLUY30fuY>
* <mark style="color:green;">systemctl</mark> daemon-reload <mark style="color:blue;">-></mark> To ensure that systemd reload with the new configuration.
* <mark style="color:green;">systemctl</mark> enable --now \<service\_name> <mark style="color:blue;">-></mark> to make the service starts autmoatically at boot.
* <mark style="color:green;">systemctl</mark> reload \<service\_name> <mark style="color:blue;">-></mark> reload the config files of a service.
* <mark style="color:green;">systemctl</mark> disable \<service\_name> <mark style="color:blue;">-></mark> to disable the service from starting automatically at boot

### Practice Time:&#x20;

1 - Make sure httpd service is automatically started.\
2 - Edit its configuration file such that on failure, it will continue after 130s.

#### Answer :&#x20;

First lets edit the unit config file of httpd service, \ <mark style="color:blue;">systemctl</mark> edit --full httpd \ <mark style="color:green;">\[service]</mark>\
Restart = always\
Restartsec= 130s\ <mark style="color:blue;">systemctl</mark> daemon-reload\ <mark style="color:red;">killall</mark> httpd\ <mark style="color:blue;">systemctl</mark> restart httpd\ <mark style="color:blue;">systemctl</mark> enable --now httpd\ <mark style="color:blue;">systemctl</mark> status httpd\ <mark style="color:blue;">systemctl</mark> cat httpd.service -> to verify the changes.<br>
