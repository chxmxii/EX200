# Managing Tuned Profiles

Tuned is a service which monitors the system and optimizes the performance of the system for different use cases. There are pre-defined tuned profiles which are present on path `/usr/lib/tuned`. Tuned profiles are designed keeping in mind three parameters linked closely to performance of the system:

- High throughput
- Low latency
- Saving power

## Recommended Manual Pages

- `man tuned`
- `man tuned-adm`
- `man tuned.conf`
- `man tuned-profiles`

## How to Use the Tuned Service

- Make sure it is running: `systemctl status tuned`
- `tuned-adm` is the CLI tool for managing profiles
- `tuned-adm list` shows available profiles
- `tuned-adm profile powersave` sets the powersave profile
- `tuned-adm active` shows the current profile

Example workflow:

```bash
systemctl status tuned
tuned-adm list
tuned-adm profile powersave
tuned-adm active
```
