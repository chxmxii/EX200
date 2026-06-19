# Managing Processes

Everything in Linux runs as a process, including services and shell commands running on a terminal. The opened terminal itself is a process. A process in Linux is a running program. A process can either run as a shell, a job, or in the background.

The command used to view all the current processes running on a Linux system is `ps`, which means "print snapshot" of the current processes. `ps` will show the running processes of the particular user, but you would need to pass the command with options many times.

- `ps` — Show current processes
- `ps L` — Show format specifiers
- `ps ef` — Show all processes
- `ps aux` — Show all processes
  - The `USER` field represents the user that is running the process
  - The `PID` field represents the process ID and it is unique
  - The `%CPU` field represents the amount of the CPU the process is using in %
  - The `%MEM` field represents the amount of the memory the process is using in %
  - The `VSZ` field represents the virtual memory size. The virtual memory size is the memory size the process has reserved and has access to but not currently using it.
  - The `RSS` field represents the resident memory size or resident set size. The resident memory is the memory size that has been allocated to the process in RAM (i.e., physical RAM size the process is using). The RSS field is also represented as `RES` if the `top` command is used.
  - The `TTY` field represents the terminal the process is running on. For a process that is running in the background, you will see "?" but for a process that is not running in the background, you will see the terminal in the form of "pts/0".
  - The `STAT` field represents current process states.
- `ps fax` — Show processes hierarchical relations
- `ps fU <username>` — Show current processes running by a specific user
- `ps eo pid,ppid,user,cmd` — Show processes with specific specifiers
- `ps -f --forest -C <process_name>` — Show a process tree for a specific process

**Note:** Consult `man ps` to learn about the alphabets representation of the process state.

- `ps -ef | grep <PID>` — Find the process name of a given PID
- `pstree | grep httpd` — See the tree of the httpd process
- `ps -auxf | sort -nr -k 3 | head -10` — View the top 10 CPU consuming processes

## CPU

Some useful commands:

- `w <user_name>` — Learn about a user's activity and what they are doing
- `uptime` — Check how long the system has been running
- `lscpu` — View information about your CPU architecture

## Jobs

A job cannot be made persistent after a reboot. It just runs on the Linux shell. A job can be suspended, stopped, or continued.

Some jobs keep the shell busy for a while, making the admin unable to continue their work until the job completes. One of the jobs that takes a long time is using the `dd` utility to make a flash bootable, especially an ISO that is about 8 GiB.

Example of a dummy command using the `dd` utility:

```bash
dd if=/dev/zero of=/dev/null
```

The command keeps the shell busy and will not release the cursor. As a user, you can make this kind of job run in the background so that you can continue to do other things.

- `Ctrl + Z` then `bg` — Make the job run in the background
- `command &` — Add `&` at the end of your command to run it in the background
- `jobs` — Display the running commands in the background
- `fg n` — Move job number n to the foreground

## How to Kill a Process

There are situations where an application will stop responding and you may have done all the necessary things to stop and start the application but it never comes up because the real process is still running and was never stopped. The only way to resolve this sometimes is to kill the process by sending a signal.

**Note:** Consult `man 7 signal` to learn about the different types of signals.

- `kill -9 <PID>` — Kill a process using its PID
- `pkill -9 <process_name>` — Kill a process by the process name
- `killall -9 <process_name>` — Kill all processes matching the name

Once you kill the parent process, the child process will die too.

## Process Priority

Every process, when started, is assigned the same process priority. They run with almost the same amount of system resources except in a few cases.

You can `nice` a process, giving it a higher priority than other processes by assigning a higher amount of system resources to the process. If a process is slow or taking time to complete, you can also `renice` the process, assigning a higher amount of system resources to it.

Nicing and renicing a process (setting and changing the priority of a process) is simply giving more CPU time to a process than others. The priority can be either positive or negative.

Nice value ranges from **-20 (highest priority) to +19 (lowest priority)**.

- `top` — Display the top processes using the most processing resources
- `nice -n <nice_value> <command>` — Give a command a nice value before running it
- `renice -n <nice_value> <pid>` — Give a process a new nice value using its PID
- `renice -n <nice_value> -u <username>` — Renice all processes for a user

Another way to renice: run `top` and then press `r` to renice a process. Make sure to specify the PID.

## Tuned

As a system administrator, you can use the `tuned` application to optimize the performance profile of your system for a variety of use cases.

```bash
yum -y install tuned
systemctl enable --now tuned
tuned-adm
```

### Practice

Find a process name using its process ID.

---

```bash
ps -ef | grep <pid>
```
