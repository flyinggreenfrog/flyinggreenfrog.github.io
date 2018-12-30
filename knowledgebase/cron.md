---
title: Cron / Anacron
last-changed: <time>2018-12-30</time>
knowledgebase: true
categories: [Linux]
---
## cronie / cronie-anacron

``` sh
# zypper in cronie cronie-anacron
```

``` sh
$ man 8 cron
$ man 1 crontab
$ man 5 crontab
$ man 8 anacron
$ man 5 anacrontab
```

* `/etc/crontab`
  - System crontab
  - Should be empty nowadays: anacron runs jobs
* `/etc/cron.d`
  - System cronjobs for different users
* `/var/spool/cron/tabs/<USER>`
  - User cronjobs (`crontab` command) for different users
* `/usr/lib/cron/run-crons`
  - Looks into `/etc/cron.{hourly,daily,weekly,monthly}` for scripts to run
  - Uses `/var/spool/cron/lastrun/*` to store timestamp files
  - Can be configured by `/etc/sysconfig/cron`
* `/etc/cron.hourly/0anacron`
  - Runs `anacron -s`
  - Will be run by `run-crons`
* Anacron: `/etc/anacrontab`
* Anacron stores timestamp files under `/var/spool/anacron/*`

## cron

### System crontab or cronjobs

Entry for root can start with `-`, that prevents syslog message for this
command.

``` text
# /etc/crontab or /etc/cron.d/<FILE>

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# Example of job definition:
# .---------------- minute (0-59)
# |  .------------- hour (0-23)
# |  |  .---------- day of month (1-31)
# |  |  |  .------- month (1-12) OR jan,feb,mar,apr,...
# |  |  |  |  .---- day of week (0-7) (sun=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * <USER> <CMD>

# crontab must end with newline (e.g. have an empty last line)
```

### User crontabs

``` sh
$ crontab -l
# crontab [-u user]
$ crontab -e
$ crontab -r
```

``` text
# /var/spool/cron/tabs/<USER>

# Almost same format as above for system crontab or cronjobs, only omit <USER>
```

Allow running cron jobs for user

1. `/etc/cron.allow` exists
   -> user must be present in it
2. `/etc/cron.allow` doesn't exist, but `/etc/cron.deny` exists
   -> user must not be present in it
3. Neither file exist -> only root can use it

## anacron

### anacrontab

`/etc/anacrontab`
``` text
# /etc/anacrontab

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
RANDOM_DELAY=45
START_HOURS_RANGE=3-22
#Days     Delay in min  Job-Id        Command
@daily    15            test.daily    <CMD>
```
