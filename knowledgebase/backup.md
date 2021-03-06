---
title: Backup
last-changed: <time>2020-04-24</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Borg - Deduplicating Archiver](https://www.borgbackup.org) <time>2019-06-17</time>
* [Borg Documentation](https://borgbackup.readthedocs.io) <time>2019-06-17</time>
* [Borg - Linux Backup Tool featured Deduplicating, Compression and Encryption](https://linoxide.com/linux-how-to/borg-backup-linux-tool) <time>2019-06-17</time>
* [System- und Dateibackup mit Borg](https://www.pro-linux.de/artikel/2/1918/system-und-dateibackup-mit-borg.html) <time>2019-06-17</time>
* [The Tao of Backup](http://www.taobackup.com) <time>2019-06-17</time>

## General

Run backup program as root (to save all files from all owners, that is to avoid
read permission problems).

Paths to separately backup:

* `/` (without `/home/*` and `/srv/*`)
* `/srv`
* `/home`

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

## borg

Help:

```sh
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

```sh
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

```sh
# borg init --encryption repokey|keyfile|repokey-blake2|keyfile-blake2
```

Make backup:

```sh
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

```sh
# borg check \
  --verbose \
  --show-rc
```

List backups:

```sh
# borg list
# borg list ::'<ARCHIVE>'
# borg list -P|--prefix <PREFIX>
```

Delete old backups:

```sh
# borg -v delete ::'<ARCHIVE>'
# borg -v delete -P|--prefix <PREFIX>
```

Mount backup (partial restore):

```sh
# borg mount --verbose [-P|--prefix <PREFIX>] :: /mnt
# borg mount --verbose ::'<ARCHIVE>' /mnt
# ls /mnt
# borg umount /mnt
```

Extract (borg extracts normally to current working dir):

```sh
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

```sh
# borg key export --qr-html :: <PAPERKEYFILE>
# borg key export --paper :: <PAPERKEYFILE>
# borg key export :: <KEYFILE>
```

Restore backup encryption key:

```sh
# borg key import --paper ::
# borg key import :: <KEYFILE>
```

Change passphrase:

```sh
# borg key change-passphrase -v ::
# BORG_PASSPHRASE=<OLD> BORG_NEW_PASSPHRASE=<NEW> borg key change-passphrase ::
```
