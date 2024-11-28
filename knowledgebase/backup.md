---
title: Backup
last-changed: <time>2023-02-17</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Borg - Deduplicating Archiver](https://www.borgbackup.org) <time>2023-02-17</time>
* [Borg Documentation](https://borgbackup.readthedocs.io) <time>2023-02-17</time>
* [borgmatic](https://torsion.org/borgmatic) <time>2023-02-17</time>
* [Borg - Linux Backup Tool featured Deduplicating, Compression and Encryption](https://linoxide.com/linux-how-to/borg-backup-linux-tool) <time>2023-02-17</time>
* [System- und Dateibackup mit Borg](https://www.pro-linux.de/artikel/2/1918/system-und-dateibackup-mit-borg.html) <time>2023-02-17</time>
* [The Tao of Backup](http://www.taobackup.com) <time>2023-02-17</time>

## General

Run backup program as root (to save all files from all owners, that is to avoid
read permission problems).

Paths to separately backup:

* `/` (without `/boot/*`, `/home/*` and `/srv/*`)
* `/boot`
* `/home`
* `/srv`

Excludes:

```text
/dev/*
/proc/*
/run/*
/sys/*
/tmp/*
/var/cache/*
/var/run/*
/var/tmp/*
/srv/backup/*
/media/*
/mnt/*
/.snapshots/*
/root/.cache/*
/home/*/.cache/*
```

Don't backup BTRFS `/.snapshots` due to size problems with restore.

## borgmatic

```console
# findmnt /media/REAR-003
# mkdir -pv /media/REAR-003/backup/borg
# cp /etc/borgmatic.d/{hetzner,usb-wd}.yaml
# borgmatic -c /etc/borgmatic.d/usb-wd.yaml init -e keyfile-blake2

# # Save to gopass on controller
# borgmatic -c /etc/borgmatic.d/usb-wd.yaml key export
# # Copy and paste

# # do backup
# borgmatic -c /etc/borgmatic.d/usb-wd.yaml prune compact create --verbosity -1 --syslog-verbosity 1
```



## borg

Help:

```console
# borg help compression
# borg help patterns
# borg help placeholders
```

Comparisions for compression:

```text
total backuped size 5.6G

lzma,6 -> 1.7G in 34 min
none -> 4.9G in ~5 min
lz4 -> 2.7G in ~4 min
zstd,3 -> 2.0G in ~4 min

For me I choose zstd,3 from now on.
```

Set some environment variables, that borg commands will use:

```console
# export BORG_PASSPHRASE='<SECRET>'
# export BORG_REPO='backup@<SERVER>:borgbackup/<HOSTNAME>'
# export BORG_RSH='ssh -oBatchMode=yes'
```

#### `<SERVER>:/home/backup/.ssh/authorized_keys`

```text
command="borg serve --restrict-to-path <PATH>",restrict ssh-rsa <KEY> <COMMENT>
command="borg serve --restrict-to-path <PATH> --append-only",restrict ssh-rsa <KEY> <COMMENT>
```

Initialize repo:

```console
# borg init --encryption repokey|keyfile|repokey-blake2|keyfile-blake2
```

Make backup:

```console
# borg create \
  --verbose \
  --stats \
  --list \
  --show-rc \
  --compression zstd \
  --filter=AME \
  --exclude-caches \
  --exclude-if-present .nobackup \
  --exclude-from /etc/borg/exclude.txt \
  ::'{hostname}_{now:%Y-%m-%d_%H%M%S}' \
  <PATHS>
# borg prune \
  --verbose \
  --stats \
  --list \
  --show-rc \
  --keep-within 2d \
  --keep-daily 7 \
  --keep-weekly 4 \
  --keep-monthly 12 \
  --keep-yearly 2 \
  --prefix '{hostname}_'
```

Check all backups:

```console
# borg check \
  --verbose \
  --show-rc
```

List backups:

```console
# borg list
# borg list ::'<ARCHIVE>'
# borg list -P|--prefix <PREFIX>
```

Delete old backups:

```console
# borg -v delete ::'<ARCHIVE>'
# borg -v delete -P|--prefix <PREFIX>
```

Mount backup (partial restore):

```console
# borg mount --verbose [-P|--prefix <PREFIX>] :: /mnt
# borg mount --verbose ::'<ARCHIVE>' /mnt
# ls /mnt
# borg umount /mnt
```

Extract (borg extracts normally to current working dir):

```console
# cd / && borg extract \
  --sparse \
  --list \
  --show-rc \
  ::'<ARCHIVE>'
```

### Encryption

* If you trust server/media, then use `repokey|repokey-blake2`.
* If you don't trust server/media, then use `keyfile|keyfile-blake2`.
  - Keys are then under `~/.config/borg/keys/`.
  - Important: Backup your keyfile seperately!
* Backup encryption keys e.g. via paper key.
* Because of LUKS encrypted backup media, you may consider using empty
  passphrase for borg key.

Backup encryption key:

```console
# borg key export --qr-html :: <PAPERKEYFILE>
# borg key export --paper :: <PAPERKEYFILE>
# borg key export :: <KEYFILE>
```

Restore backup encryption key:

```console
# borg key import --paper ::
# borg key import :: <KEYFILE>
```

Change passphrase:

```console
# borg key change-passphrase -v ::
# BORG_PASSPHRASE=<OLD> BORG_NEW_PASSPHRASE=<NEW> borg key change-passphrase ::
```
