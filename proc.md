# Managing Processes

<mark style="color:red;">Definition :</mark> Everything in Linux runs as a process, I mean everything, including services and a shell command that is running on a terminal. As a matter of fact, the opened terminal it’s self is a process. Therefore, a process in Linux is a running program. A process can either run as a shell, a job or in the background.

The command used to view all the current processes running on Linux system is “**ps**“, which means (print snapshot) of the current processes. **ps** will show the running process of the particular user, but of course you would need to pass the command with options many times.&#x20;

* <mark style="color:yellow;">ps</mark> -> Show current processes

* <mark style="color:yellow;">ps</mark> L -> Show format specifiers

* <mark style="color:yellow;">ps</mark> ef -> Show all Processes

* <mark style="color:yellow;">ps</mark> aux -> Show all Processes

  * The “<mark style="color:yellow;">**USER**</mark>” field represents the user that is running the process
  * The “<mark style="color:yellow;">**PID**</mark>” field represents the process ID and it is unique.
  * The “<mark style="color:yellow;">**%CPU**</mark>” field represents the amount of the CPU the process is using in %
  * The “<mark style="color:yellow;">**%MEM**</mark>” represents the amount of the memory the process is using in %.
  * The “<mark style="color:yellow;">**VSZ**</mark>” field represents the virtual memory size. The virtual memory size is the memory size the process has reserved and has access to but not currently using it.
  * The “<mark style="color:yellow;">**RSS**</mark>” field represents the resident memory size or resident set size. The resident memory is the memory size that has been allocated to the process in RAM. (i.e, physical RAM size the process is using). The RSS field is also represented as “<mark style="color:yellow;">**RES**</mark>” if the top command is used.
  * The “<mark style="color:yellow;">**TTY**</mark>” field represents the terminal the process is running on. For a process that is running in the background, you will see “?” but for a process that is not running in the background, you will see the terminal, the terminal can be in the form of “pts/0”.
  * The “<mark style="color:yellow;">**STAT**</mark>” field represents current process states. The process states are as follow:

* <mark style="color:yellow;">ps</mark> fax -> Show processes hierarchical relations.

* <mark style="color:yellow;">ps</mark> fU <mark style="color:yellow;">\<username></mark> -> Shows current processes running by <mark style="color:yellow;">\<username>.</mark>

* <mark style="color:yellow;">ps</mark> eo `pid,ppid,user,cmd`-> Show processes with specific specifiers we pick.

* <mark style="color:yellow;">ps</mark> -f --forest -C <mark style="color:yellow;">\<process\_name></mark> ->  Show a process **tree** for a specific process <mark style="color:yellow;">**<**</mark>*<mark style="color:yellow;">**process\_name**</mark>*<mark style="color:yellow;">**>.**</mark>

{% hint style="info" %}
Consult man **ps** to learn about the alphabets representation of the process state.
{% endhint %}

* <mark style="color:yellow;">ps</mark>  -ef | grep <mark style="color:yellow;">\<PID></mark> ->  to find the process name of the PID 3680&#x35;**.**
* <mark style="color:yellow;">pstree</mark> | grep <mark style="color:yellow;">httpd</mark> ->  To see the tree of the httpd process.
* <mark style="color:yellow;">ps</mark> -auxf | <mark style="color:yellow;">sort</mark> -nr -k 3 | <mark style="color:yellow;">head</mark> -10 -> View the top 10 CPU Consuming Process.

### CPU :&#x20;

some useful commands :&#x20;

* <mark style="color:blue;">w</mark> \<user\_name> <mark style="color:blue;">-></mark> Learn about some user's  head activity and what are they doing.
* <mark style="color:blue;">uptime</mark> -> to check how long the system has been running.
* <mark style="color:blue;">lscpu</mark> -> View different informations about your CPU architecture

## Jobs :&#x20;

A job cannot be made persistent after a reboot. It just runs on the Linux shell. but a job can be  " suspended <mark style="color:blue;">/</mark> stopped <mark style="color:blue;">/</mark> continued ".\
here are some jobs that keep the shell busy for a while, thereby making the admin redundant until the job gets completed.\
The admin cannot continue their work except another terminal is opened. One of the Jobs that take a long time is using the dd utility to make a flash boo-table, especially an ISO that is about 8 GiB. I know how much time this will takes .\
Lemme make it clear for you, am going to run a dummy command using the dd utility.

```
dd if=/dev/zero of=/dev/null
```

you can see that the command keeps the shell busy and won’t release the cursor. As a user, you can make this kind of job run in the background so that you can continue to do other things.

* <mark style="color:red;">ctrl + Z</mark> then <mark style="color:red;">bg</mark> -> to make the job run in the background.
* command<mark style="color:red;">&</mark> -> add "&" in the end of your command to run it in the background.
* <mark style="color:red;">jobs</mark> -> to display the running command in the background.
* <mark style="color:blue;">fg</mark> n -> to move the job to the foreground.

## How to kill a process :&#x20;

There are some situations where an application will stop responding and you may have done all the necessary things to stop and start the application but it never comes up because the real process is still running and was never stopped in the real sense, the only way to come out from such sometimes is to kill the process by sending a signal.

![](/files/QyRcGQzXIOJyE7hN2Kkh)

{% hint style="info" %}
consult **man 7 signal** to learn about the different types of signals.
{% endhint %}

* <mark style="color:red;">kill</mark> -9 \<PID> -> to <mark style="color:red;">**kill**</mark> a process using\<pid>.
* <mark style="color:red;">pkill</mark> -9 \<process\_name> -> to <mark style="color:red;">kill</mark> a  process by the process name.
* <mark style="color:red;">killalll</mark> -9 \<process\_name> -> Same thing as ^.

**=>** once you <mark style="color:red;">kill</mark> the <mark style="color:green;">parent process</mark> is killed. <mark style="color:green;">the child process</mark> will DIEEEEE too.

## Process Priority : &#x20;

<mark style="color:red;">E</mark>very process, when they are started are assigned the same process priority. i.e, they run with almost the same amount of system resources except in a few cases.\
you can **nice** such process. i.e, give it a higher priority than other processes by assigning a higher amount of system resources to the process.\
More so, if a process is slow or taking time to complete, it could be a job as well, you can also **re-nice** the process. i.e, assign a higher amount of system resources to the process.\
**Nicing** and re-**nicing** a process, in other words, setting and resetting/changing the priority of a process is simply giving more CPU time to a process than the other and it can either be positive or negative\ <mark style="color:red;">=></mark> nice value ranges from **-20 (highest priority) to +19 (lowest priority).**

* <mark style="color:red;">top</mark> -> display the top processes that are using more higher processing resources.
* <mark style="color:red;">nice</mark> -n <mark style="color:yellow;">\<nice\_value></mark> <mark style="color:red;">\<command></mark> -> to give a command a nice value before running it.
* <mark style="color:red;">renice</mark> -n \<nice\_value> <mark style="color:red;">\<pid></mark> / -u <mark style="color:red;">\<username></mark> -> to give the process a new nice value. either with \<pid> or the \<username>.\
  \=> <mark style="color:red;">A</mark>nother way to renice : run the command "<mark style="color:red;">**top**</mark>" and then press on "<mark style="color:blue;">**r**</mark>" button to renice a process, make sure to precise the pid.

## Tuned:&#x20;

As a system administrator, you can use the **TuneD** application to optimize the performance profile of your system for a variety of use cases.

* yum -y install tune**d**
* systemctl enable --now tuned
* tuned-adm

{% hint style="success" %}
ps -ef | grep \<pid> -> to find a process name with process ID.

Otherwise, great job. lets move on to the next chapter.
{% endhint %}
