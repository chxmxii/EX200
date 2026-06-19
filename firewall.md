# Firewalling

**Firewalld** uses different components to make firewalling easier

* <mark style="color:blue;">Service:</mark> the main component, contains 1 or more ports as well as optional kernel modules that should be loaded.
* <mark style="color:blue;">Zone:</mark> a default configuration to which network cards can be assigned to apply specific settings (internal, external)
* <mark style="color:blue;">Ports:</mark> optional elements to allow access to specific ports (just use services instead, it's more convenient)

firewall-cmd :  ⦁ --reload -> <mark style="color:yellow;">to reload firewalld serivce</mark>\
&#x20;                        ⦁ --get-zones -> <mark style="color:yellow;">List all the zones</mark>\
&#x20;                        ⦁ --get-default-zone -> <mark style="color:yellow;">display the default zone</mark>\
&#x20;                        ⦁ --set-default-zone=ZONE  -> <mark style="color:yellow;">set default zone</mark>\
&#x20;                        ⦁ --get-services -> <mark style="color:yellow;">display all available services</mark>\
&#x20;                        ⦁ --list-services -> <mark style="color:yellow;">list services</mark>\
&#x20;                        ⦁ --add-service=SERVICE NAME \[--zone=ZONE] -> <mark style="color:yellow;">add new service</mark>\
&#x20;                        ⦁ --remove-service= SERVICE NAME -> <mark style="color:yellow;">remove service</mark>\
&#x20;                        ⦁ --add-port=PORT/PROTOCOL -> <mark style="color:yellow;">add port</mark>\
&#x20;                        ⦁ --remove-port=PORT/PROTOCOL -> <mark style="color:yellow;">remove port</mark>\
&#x20;                        ⦁ --add-interface=INTERFACE -> <mark style="color:yellow;">add interface</mark> \
&#x20;                        ⦁ --remove-interface=INTERFACE -> <mark style="color:yellow;">remove interface</mark>\
&#x20;                        ⦁ --add-source=IP ADD/ MASK -> a<mark style="color:yellow;">dd an IP source</mark>\
&#x20;                        ⦁ --remove-source=ip/mask -> <mark style="color:yellow;">remove source.</mark>\
&#x20;                        ⦁ --permanent -> <mark style="color:yellow;">to set-add-remove {service-port-zone} permanently.</mark>

You can use the GUI interface too.\ <mark style="color:green;">`yum install firewall-config -y`</mark>
