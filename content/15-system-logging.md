# System Logging

Most services used on a Linux server write information to log files. This information can be written to different destinations, and there are multiple solutions to find the relevant information in system logs.

## Logging Approaches

Many different approaches can be used by services to write log information. The most common are:

- **Direct Writing** — Services write directly to log files. For example, using `logger -p kern.err "hello"` to write a kernel error message.
- **rsyslogd** — An enhancement of syslogd, a service that takes care of managing centralized log files. Syslogd has been around for a long time.
- **journald** — With the introduction of systemd, the `systemd-journald` log service has been introduced as well. This service is tightly integrated with systemd, which allows administrators to read detailed information from the journal while monitoring service status using the `systemctl status` command.

You can find all global system activities within `/var/log/syslog`, while security-related events are stored in `/var/log/audit.log`.

## systemd-journald

Useful commands for working with the journal:

```bash
journalctl -u <service-name>
```

See logs about a specific service.

```bash
journalctl -f
```

Follow the journal in real time until you stop it.

```bash
journalctl -f --no-pager -p info --since yesterday -o verbose _SYSTEMD_UNIT=sshd.service
```

Show verbose info-level messages from `sshd.service` since yesterday.

### Persistent Journal Storage

By default, the journal is not persistent. To make it persistent, modify `/etc/systemd/journald.conf`:

```bash
vim /etc/systemd/journald.conf
```

Add or set the following line:

```
Storage=persistent
```

Then restart the service:

```bash
systemctl restart systemd-journald
```

**Note:** `Storage=persistent` will create `/var/log/journal`. Setting `Storage=auto` will only store logs if `/var/log/journal` already exists, so make sure to create that directory if you use `Storage=auto`. Consult the man page for more info.

## rsyslog

rsyslog is the service that takes care of managing centralized log files. To make sure that the information you need is logged in a location where you want to find it, you can configure the rsyslog service through `/etc/rsyslog.conf`.

### Configuration Sections (Modules, Global Directives, Rules)

There are different sections inside `/etc/rsyslog.conf` that allow you to specify where and how information should be written:

- **Modules** — rsyslog is modular. Modules are included to enhance the supported features.
- **Global Directives** — Used to specify global parameters, such as the location where auxiliary files are written or the default timestamp format.
- **Rules** — The most important part. Used to specify what information should be logged to which destination.

### Facilities and Priorities

The rsyslog service uses facility, priority, and log destination:

- **Facility** is the category of the message.
- **Priority** defines the severity level.
- **Log destination** defines the location where messages are written.

Example rule:

```
local1.error    /var/log/httpd-error.log
```

This sends messages from facility `local1` with priority `error` or higher to `/var/log/httpd-error.log`.

## logrotate

Log rotation is used to prevent syslog messages from filling up your system. `logrotate` allows you to delete or compress log files when they reach a specified condition (size, age, etc.).

The logrotate configuration file is located at `/etc/logrotate.conf`. Consult `man logrotate` for more help.
