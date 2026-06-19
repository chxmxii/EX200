# Managing Tuned Profiles.

<mark style="color:blue;">T</mark>uned is service which monitors the system and optimizes the performance of system for different use cases. \ <mark style="color:blue;">T</mark>here are pre-defined tuned profiles which are present on path /usr/lib/tuned.\ <mark style="color:blue;">T</mark>uned profiles are designed keeping in mind three parameters linked closely to performance of system. \ <mark style="color:yellow;">• High throughput .</mark>\ <mark style="color:yellow;">• Low latency.</mark> \ <mark style="color:yellow;">• Saving power.</mark>\
Recommended manual Pages : \ <mark style="color:yellow;">• man tuned</mark> \ <mark style="color:yellow;">• man tuned-adm</mark> \ <mark style="color:yellow;">• man tuned.conf</mark> \ <mark style="color:yellow;">• man tuned-profiles</mark><br>

### How to use tuned service :&#x20;

* Make sure its running <mark style="color:green;">`systemctl status tuned`</mark>
* <mark style="color:green;">`tuned-adm`</mark> is the CLI
* <mark style="color:green;">`tuned-adm list`</mark> show available profiles
* <mark style="color:green;">`tuned-adm profile powersave`</mark> to set the powersave profile
* <mark style="color:green;">`tuned-adm active`</mark> show current profile

![](/files/jh20yIqFIhRbEBOlosWq)
