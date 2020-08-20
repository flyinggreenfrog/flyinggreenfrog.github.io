---
title: ReaR
last-changed: <time>2020-08-19</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Relax-and-Recover](http://relax-and-recover.org) <time>2020-04-28</time>
* [Disaster Recovery](https://en.opensuse.org/SDB:Disaster_Recovery) <time>2020-04-28</time>

## Terms

ReaR
: Relax and Recover

## Setup

```sh
# zypper in rear
```

## Configuration

All config variables are described in `/usr/share/rear/conf/default.conf`.

Config files are found under `/etc/rear`.

#### `local.conf`

```text
# Manual configuration
```

#### `site.conf`

```text
# Use one of `<MYCONF>.conf` explicitely via `rear -C <MYCONF>`
OUTPUT=USB
USB_DEVICE=''

REQUIRED_PROGS+=(
  lsblk
  shred
  lsinitrd
  xz
  xzcat
  mkinitrd
)
```

#### `usb-usb-all.conf`

```text
OUTPUT=USB
USB_DEVICE=/dev/disk/by-label/REAR-000
USB_RETAIN_BACKUP_NR=10

BACKUP=BORG

COPY_AS_IS+=(
  /etc/borg/all.rc
  /etc/borg/system.rc
  /etc/borg/home.rc
)

### Backup includes

BACKUP_ONLY_INCLUDE='yes'
BACKUP_PROG_INCLUDE=(
  '/'
  '/boot'
  '/home'
)

### Backup excludes

BACKUP_PROG_EXCLUDE+=(
  '/dev/*'
  '/proc/*'
  '/run/*'
  '/sys/*'
  '/tmp/*'
  '/var/cache/*'
  '/var/run/*'
  '/var/tmp/*'
  '/srv/backup/*'
  '/srv/repo/dvd-*'
  '/media/*'
  '/mnt/*'
  '/.snapshots/*'
  '/root/.cache/*'
  '/home/*/.cache/*'

### SSH settings

# Set password for accessing the recovery system itself.
SSH_ROOT_PASSWORD=<PASSWORD>

### Settings for borgbackup

source "/etc/borg/${MYCONF%.*}.rc"

# We leave BORGBACKUP_HOST unset, since we want to backup locally to USB.
#BORGBACKUP_HOST=
BORGBACKUP_REPO="/borg_repos/$HOSTNAME"

BORGBACKUP_ARCHIVE_PREFIX='all'
BORGBACKUP_ENC_TYPE=repokey-blake2
BORGBACKUP_COMPRESSION='zstd,3'

#BORGBACKUP_SHOW_PROGRESS='no'
BORGBACKUP_SHOW_STATS='yes'
BORGBACKUP_SHOW_LIST='yes'
BORGBACKUP_SHOW_RC='yes'
BORGBACKUP_EXCLUDE_IF_NOBACKUP=True
BORGBACKUP_INIT_MAKE_PARENT_DIRS=True

BORGBACKUP_PRUNE_WITHIN=2d
BORGBACKUP_PRUNE_LAST=10
#BORGBACKUP_PRUNE_HOURLY=
#BORGBACKUP_PRUNE_DAILY=
#BORGBACKUP_PRUNE_WEEKLY=
#BORGBACKUP_PRUNE_MONTHLY=
#BORGBACKUP_PRUNE_YEARLY=
```

#### `usb-usb-system.conf`

Similar to `usb-usb-all.conf`:

```text
[...]

BACKUP_PROG_INCLUDE=(
  '/'
  '/boot'
)

BACKUP_PROG_EXCLUDE=(
  '/srv'
  '/home'
)

[...]
```

#### `usb-usb-srv.conf`

Similar to `usb-usb-all.conf`:

```text
[...]

BACKUP_PROG_INCLUDE=(
  '/srv'
)

[...]
```

#### `usb-usb-home.conf`

Similar to `usb-usb-all.conf`:

```text
[...]

BACKUP_PROG_INCLUDE=(
  '/home'
)

[...]
```

#### `iso-ssh-all.conf`, `iso-ssh-system.conf`, `iso-ssh-srv.conf`, `iso-ssh-home.conf`

Similar to `usb-usb-all.conf`, `usb-usb-system.conf`, `usb-usb-srv.conf`, `usb-usb-home.conf`:

```text
[...]

### SSH settings

# Set password for accessing the recovery system itself.
SSH_ROOT_PASSWORD=<PASSWORD>
# For accessing the borg repo copy ssh keys to recovery system.
SSH_FILES='yes'
# Since we save the ISO itself to an encrypted borg repo,
# it's save to include passwordless ssh private keys.
SSH_UNPROTECTED_PRIVATE_KEYS='yes'

### Settings for borgbackup

source "/etc/borg/${MYCONF%.*}.rc"

BORGBACKUP_HOST=<SERVER>
BORGBACKUP_USER=<REMOTEUSER>
BORGBACKUP_REPO="<PATH>/$HOSTNAME"

[...]
```

## Usage

`mkbackup = mkrescue + mkbackuponly`

### USB - create

Prepare USB device as rear (and backup) medium:

```sh
# rear -v format <DEV>
```

Making rescue medium with included backup:

```sh
# rear -v -C usb-usb-all mkbackup
```

Making multiple backups together with rescue medium:

```sh
# rear -v -C usb-usb-system mkbackuponly
# rear -v -C usb-usb-home mkbackuponly
# rear -v -C usb-usb-system mkrescue
```

### ISO - create

Making rescue medium with included backup:

```sh
# rear -v -C iso-ssh-all mkbackup
```

Making multiple backups together with rescue medium:

```sh
# rear -v -C iso-ssh-system mkbackuponly
# rear -v -C iso-ssh-srv mkbackuponly
# rear -v -C iso-ssh-home mkbackuponly
# rear -v -C iso-ssh-system mkrescue
```

### Verification

Verify setup:

```sh
# rear -v -C usb-usb-system|iso-ssh-system dump
# rear -v -C usb-usb-system|iso-ssh-system checklayout
```

### Recover

* With USB medium you can boot directly.
* For ISO you have to prepare a resuce medium (ISO to USB) first.

```sh
# isohybrid [-u|--uefi] <ISO>
# dd if=<ISO> of=<DEV> status=progress bs=10M
```

If you have backuped the ISO itself with borg, you have to extract the ISO:

```sh
# borg list ::<REARISOARCHIVE>
# borg extract --stdout ::<REARISOARCHIVE> root/rear-<HOSTNAME>.iso | dd of=<DEV> status=progress bs=10M
```

After booting from rescue medium (USB or ISO), work either from rescue system
directly or connect via SSH to it. Therefore boot with adding `noip` to kernel
commandline, afterwards get IP via `dhclient`:

```sh
[...] noip
REAR# dhclient eth0
REAR# ip a
```

Connect via SSH, where resuce system has new host keys (USB):

```sh
$ ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no root@<ROOT-IP>
```

Connect via SSH, where resuce system has unchanged host keys (ISO):

```sh
$ ssh root@<REAR-IP>
```

Prepare recover:

```sh
REAR# vim /etc/borg/.passphrase.rc
export BORG_PASSPHRASE='<PASSPHRASE>'
```

Recover with single backup:

```sh
REAR# rear -v -C usb-usb-all|iso-ssh-all recover
```

Recover with multiple backups:

```sh
REAR# rear -v -C usb-usb-system|iso-ssh-system recover
REAR# rear -v -C usb-usb-home|iso-ssh-home restoreonly
```

Verify restore, e.g. if LUKS setup is correctly done:

```sh
REAR# cat /mnt/local/etc/crypttab
REAR# lsinitrd /mnt/local/boot/initrd -f /etc/crypttab
```

Reboot:

```sh
REAR# reboot
```
