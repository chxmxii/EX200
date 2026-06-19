# Working With Systemd

## Systemd

Systemd is the master of everything and the first process/service (PID = 1) to start after the Linux kernel boot. Systemd starts up and supervises almost all system units:

- Services
- Mounts
- Sockets
- Targets

Use `systemctl -t help` to list all unit types.

- `systemd-analyze blame` — shows what takes time on boot
- `systemd-analyze security` — checks the security of a unit

## Systemd Files

- `/usr/lib/systemd/system` — default unit files installed from RPM packages
- `/etc/systemd/system` — custom unit files
- `/run/systemd/system` — unit files automatically generated at runtime

**Note:** Prevent modifying a unit file in `/usr/lib/systemd/system` unless you know what you are doing.

## Systemd Unit Status

A unit status can be: Loaded, Active, Running, Exited, Waiting, Dead, Enabled, Disabled, or Static.

## Systemctl

`systemctl` is the command used to manage all units through systemd. Below are the essential commands for managing units.

```bash
systemctl --type=service --all          # list all services (active and inactive)
systemctl list-units                    # display all active units that systemd knows about
systemctl --failed --type=service       # list failed services
systemctl status -l <service_name>      # show detailed status of a service
systemctl cat <service_name>            # show current config of a unit
systemctl show <service_name>           # show available configuration options
systemctl edit --full <service_name>    # modify the default configuration
systemctl daemon-reload                 # reload systemd with the new configuration
systemctl enable --now <service_name>   # start and enable the service at boot
systemctl reload <service_name>         # reload the config files of a running service
systemctl disable <service_name>        # disable the service from starting at boot
```

### Practice

1. Make sure the `httpd` service is automatically started.
2. Edit its configuration file such that on failure, it will restart after 130 seconds.

---

```bash
systemctl edit --full httpd
```

Add the following under the `[Service]` section:

```
Restart=always
RestartSec=130s
```

Then apply and verify:

```bash
systemctl daemon-reload
killall httpd
systemctl restart httpd
systemctl enable --now httpd
systemctl status httpd
systemctl cat httpd.service   # verify the changes
```
