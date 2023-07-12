+++
categories = ['Server', 'Linux']
date = '2023-07-12'
description = 'Automatically run scripts at a specific time'
tags = ['Server', 'Debian', 'Ubuntu', 'Linux', 'Cron']
title = 'Run scripts at a specific time'
lastModifierDisplayName = "Keyvan Atashfaraz"
lastModifierEmail = "keyvan@atashfaraz.de"
+++

This is a guide to automatically run scripts at a specific time.

You need to run only one command:

```bash
crontab -e
```

This will open the crontab file in your default editor. There you can add the following line:

```bash
1 2 3 4 5 execuatable
```
1. Minutes (0-59 or *)
2. Hours (0-23 or *)
3. Days (1-31 or *)
4. Month (1-12 or *)
5. Day of the week(1-7 or *)

The executable can be a script or a command. For example:

```bash
0 0 * * * /home/user/backup.sh
```

This will run the script backup.sh every day at midnight.

Remember to make the script executable:

```bash
chmod +x /home/user/backup.sh
```
