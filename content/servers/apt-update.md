+++
categories = ['Server', 'Linux']
date = '2023-07-12'
description = 'Automatically run apt update and apt upgrade'
tags = ['Server', 'apt', 'Debian', 'Ubuntu', 'Linux', 'Cron']
title = 'apt update and apt upgrade every day'
lastModifierDisplayName = "Keyvan Atashfaraz"
lastModifierEmail = "keyvan@atashfaraz.de"
+++

This is a guide to automatically run apt update and apt upgrade.

# Cronjob: apt update and apt upgrade

You need to modify three files:
- `/lib/systemd/system/apt-daily.timer`
- `/lib/systemd/system/apt-daily-upgrade.timer`
- `/etc/cron.daily/apt-compat`

After the files are modified as described below you need to restart the systemd daemon with `systemctl daemon-reload`. When this is done your system will automatically update and upgrade the packages.

# apt-daily.timer

This timer is responsible for `apt update`. The update will update the package lists.

The file should look like this:

```ini
[Timer]
OnCalendar=*-*-* 2:18:00
# RandomizedDelaySec=12h
Persistent=true
```

The `OnCalendar` is the time when the timer is executed.  You could specify the time with the following format: `YYYY-MM-DD HH:MM:SS`.

The `RandomizedDelaySec` is the time when the timer is executed with a random delay. Through randomness you could prevent an big overload at a specific time on the mirrors. This line should be commented out, because we want to ensure that the update is executed before the upgrade. For protecting the mirrors you should use a time which is not exactly at the full, half or quarter hour.

The `Persistent` flag is important, because it makes sure that the timer is executed even if the system was down at the time when the timer was supposed to be executed.

# apt-daily-upgrade.timer

This timer is responsible for `apt upgrade`. The upgrade will upgrade the packages based on the package lists.

The file should look like this:

```ini
[Timer]
OnCalendar=*-*-* 03:18:00
# RandomizedDelaySec=60m
Persistent=true
```

The explanation for the lines are the same as for the `apt-daily.timer`.

It is important that the `OnCalendar` is after the `OnCalendar` of the `apt-daily.timer`. Otherwise the upgrade will be executed before the update.

# apt-compat

In this file you need to comment out the line which only contains `random_sleep`, but not the line which defines the `random_sleep` function.
