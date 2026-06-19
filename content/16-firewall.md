# Firewalling

`firewalld` uses different components to make firewalling easier:

- **Service:** the main component, contains one or more ports as well as optional kernel modules that should be loaded.
- **Zone:** a default configuration to which network cards can be assigned to apply specific settings (internal, external).
- **Ports:** optional elements to allow access to specific ports (just use services instead, it is more convenient).

## firewall-cmd Options

| Option | Description |
|--------|-------------|
| `--reload` | Reload the `firewalld` service |
| `--get-zones` | List all the zones |
| `--get-default-zone` | Display the default zone |
| `--set-default-zone=ZONE` | Set default zone |
| `--get-services` | Display all available services |
| `--list-services` | List services |
| `--add-service=SERVICE [--zone=ZONE]` | Add new service |
| `--remove-service=SERVICE` | Remove service |
| `--add-port=PORT/PROTOCOL` | Add port |
| `--remove-port=PORT/PROTOCOL` | Remove port |
| `--add-interface=INTERFACE` | Add interface |
| `--remove-interface=INTERFACE` | Remove interface |
| `--add-source=IP/MASK` | Add an IP source |
| `--remove-source=IP/MASK` | Remove source |
| `--permanent` | Set, add, or remove services, ports, or zones permanently |

## GUI Configuration

You can use the GUI interface too:

```bash
yum install firewall-config -y
```
