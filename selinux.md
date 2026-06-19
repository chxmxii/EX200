# SELinux

SELinux means security-enhanced Linux, it is an advanced security feature/utility in Linux.\
SELinux cannot be replaced with antivirus, [firewall](https://tekneed.com/configure-and-manage-firewall-in-linux/), [permissions](https://tekneed.com/how-to-set-permission-in-linux-and-manage-ownership/), and [ACLs ](https://tekneed.com/how-to-manage-acl-in-linux-set-permissions-using-acl/)on Linux. As the name implies, it is a security-enhanced feature and not a replacement of other security features.\
->To show selinux context labels (<mark style="color:blue;">`ps auxZ`</mark> or <mark style="color:blue;">`ls -lZ`</mark>)\
-> <mark style="color:red;">`sestatus -v`</mark> : to check the current selinux status.\
-><mark style="color:red;">`getenforce`</mark> : same thing.\
-><mark style="color:red;">`setenforce 0`</mark> : to set the selinux in permissive mode.\
-><mark style="color:red;">`>setenforce 1`</mark>: if you want to set it to enforcing mode.\
\=> the last two commands are meant to change the SELinux temporarily but permanent. if you want to, then you have to change the SELinux configuration file which located under <mark style="color:yellow;">**/etc/sysconfig/selinux**</mark>\ <mark style="color:yellow;">**Enforcing Mode :**</mark> SELinux is fully operational and enforcing all SELinux rules in the policy\ <mark style="color:yellow;">**Permissive Mode :**</mark> all SELinux-related activity is logged, but no access is blocked.\ <mark style="color:yellow;">**Disabled Mode :**</mark> Never set to disabled, unless you know that that's what you really want.

### SELinux Contexts :&#x20;

The <mark style="color:blue;">SELinux</mark> policy uses these **contexts** in a series of rules which define how processes can interact with each other and the - various system resources. By default, the policy does not allow any interaction unless a rule explicitly grants access. \
-> SELinux contexts have several fields: <mark style="color:green;">`user, role, type, and security level`</mark>. The SELinux type information is the most important when it comes to the SELinux policy, as the most common policy rule which defines the allowed interactions between processes and system resources uses SELinux types and not the full **SELinux context. &#x20;**<mark style="color:red;">**`user_context:role_context:type`**</mark>

![](/files/1DKqvbqycBX6eyn2u9AG)

-> For example, the SELinux type name for the web server process is <mark style="color:green;">**httpd\_t.**</mark> \
->The type context for files and directories normally found in <mark style="color:green;">/var/www/html/</mark> is <mark style="color:red;">httpd\_sys\_content\_t</mark>**.**\
**->** How Context settings applied  : \
&#x20;⦁) <mark style="color:purple;">If a new file is created</mark>, it inherits the context settings from the parent directory. \
&#x20;⦁) <mark style="color:purple;">If a file is copied to a directory</mark>, this is considered a new file, so it inherits the context settings from the parent directory. \
&#x20;⦁) <mark style="color:purple;">If a file is moved</mark>, or copied while keeping its properties (by using cp -a), the original context settings of the file are applied. \ <mark style="color:green;">`Small hint : when using mv command make sure to include the -Z option, so it will inherits the context of the new directory.`</mark>\
**For better understanding the SELinux contest lets analyze the diagram below :**&#x20;

![](/files/WEzact5WgaYucSkqu8xt)

Uses the general purpose <mark style="color:yellow;">**`semanage`**</mark> to define file, port and other object contexts.\
\
-> <mark style="color:blue;">`semanage fcontext`</mark> writes a file context into the selinux policy for use.\
For file system based objects, tweaking a policy does not take affect immediately.\
->Use <mark style="color:blue;">`restorecon`</mark> to enforce a policy on the file system\
&#x20;<mark style="color:green;">e.g. semanage fcontext -a -t system\_u:object\_r:etc\_t "/etc(/.\*)?"</mark>\ <mark style="color:green;">`restorecon -Rv /etc`</mark>\
-> Another option is to <mark style="color:blue;">`touch /.autorelabel`</mark> and reboot.\
-> To see the context of a port use this command : \ <mark style="color:yellow;">`netstat -Ztulpen`</mark>

![](/files/CXaMGELoBW77WxqNTOrE)

### Booleans :&#x20;

* Higher level concept for turning on/off complete set of functionlity
* <mark style="color:blue;">`getsebool -a`</mark> list all
* To toggle a bool <mark style="color:blue;">`setsebool -P httpd_enable_homedirs on.`</mark>
* e.g : allow httpd to see home dirs for a public webpage\
  -> show all booleans and pipe to grep for httpd :

  `getsebool -a | grep httpd`

  `httpd_enable_homedirs --> off`

  ->change boolean : \
  `setsetbool -P http_enable_homedirs on`

### Logging :&#x20;

* Default uses <mark style="color:red;">`auditd`</mark>, logs are not human friendly <mark style="color:red;">`grep AVC /var/log/audit/audit.log`</mark>
* AVC = access vector cache, and is a signature of selinux logs
* Nicer is <mark style="color:blue;">`sealert`</mark> which parses raw audit log events, value adds and writes <mark style="color:blue;">`/var/log/messages`</mark>
* Run <mark style="color:blue;">`sealert <uuid>`</mark> to get advice on a known event
* Use <mark style="color:blue;">`journalctl | grep sealert`</mark> to locate UUID

![](/files/cDnvFmQSTLuoAHE8CnJL)

\==> If a service is not working, always suspect selinux.\
&#x20;     Check if its running `getenforce.`\
&#x20;     Temporarily relax to permissive mode `setenforce 0.`\
&#x20;      Re-test, if the service is operational, you know selinux is to blame.\
&#x20;      `grep sealert /var/log/messages.`
