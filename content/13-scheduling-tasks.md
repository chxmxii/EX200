# Scheduling Tasks

## Cron

Cron is a daemon that triggers jobs at specific times, mostly used for recurring jobs.
`systemd timer` is the modern alternative to cron.

- Scripts are executed on an `hourly`, `daily`, `weekly`, or `monthly` basis.
- Generic time-specific cron jobs are stored in `/etc/cron.d`.

Steps to schedule a job with cron:

1. Edit cron table:

```bash
crontab -e
```

Or create a file under `/etc/cron.d/`.

2. Check `/etc/crontab` for the format if you forget.

3. Verify jobs:

```bash
crontab -l
```

4. Remove all jobs:

```bash
crontab -r
```

5. Cron jobs for each user are stored in `/var/spool/cron`.

**Note:** You can use `@hourly`, `@daily`, etc., as shortcuts. Cron has no `STDOUT`. Use `logger` instead of `echo`. More info: `man 5 crontab`.

## Anacron

Anacron executes cron jobs regularly, but not at a specific time.
It is useful for daily, weekly, or monthly jobs (or every n days).

- Configuration file: `/etc/anacrontab`

Example `/etc/anacrontab` format:

```bash
period (days)   delay (minutes)   job-identifier   command
1   15   my.daily   /usr/bin/yum update -y
```

## at

The `at` command is used for one-time jobs.
The service `atd` runs these scheduled jobs.

Schedule a job:

```bash
at 3pm today
```

Example task:

```bash
echo "Hello World" > /root/example.txt
```

Press `CTRL+D` to save and quit the `at` interface.

List pending jobs:

```bash
atq
```

Remove a scheduled job:

```bash
atrm <job_number>
```
