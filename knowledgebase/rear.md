---
title: ReaR
last-changed: <time>2023-02-17</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Relax-and-Recover](http://relax-and-recover.org) <time>2023-02-17</time>
* [Disaster Recovery](https://en.opensuse.org/SDB:Disaster_Recovery) <time>2023-02-17</time>

## Terms

ReaR
: Relax and Recover

## Setup

```console
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
BACKUP=REQUESTRESTORE
OUTPUT=ISO

REQUIRED_PROGS+=(
  lsblk
  cfdisk
  shred
  lsinitrd
  xz
  xzcat
  mkinitrd
  borg
  borgmatic
)

SSH_FILES='avoid_sensitive_files'
# Set password for accessing the recovery system itself.
SSH_ROOT_PASSWORD=<PASSWORD>
```

#### `usb.conf`

```text
BACKUP=REQUESTRESTORE
OUTPUT=USB
USB_DEVICE=/dev/disk/by-label/REAR-001
USB_DEVICE_FILESYSTEM_LABEL=REAR-001
USB_RETAIN_BACKUP_NR=10
```

## Usage

`mkbackup = mkrescue + mkbackuponly`

With borgmatic use only mkrescue as borgmatic `before_backup hook`, since
borgmatic takes care of the backup itself.

### USB - create

Prepare USB device as rear medium:

```console
# rear -v format -C usb <DEV>
```

Making rescue medium:

```console
# rear -v -C usb mkrescue
```

### ISO - create

Making rescue medium:

```console
# rear -v mkrescue
```

### Verification

Verify setup:

```console
# rear -v [-C usb] dump
# rear -v [-C usb] checklayout
```

### Recover

* With USB medium you can boot directly.
* For ISO you have to prepare a rescue medium (ISO to USB) first.

```console
# isohybrid [-u|--uefi] <ISO>
# dd if=<ISO> of=<DEV> status=progress bs=10M
```

If you have backuped the ISO itself with borg, you have to extract the ISO:

```console
# borg list ::<REARISOARCHIVE>
# borg extract --stdout ::<REARISOARCHIVE> root/rear-<HOSTNAME>.iso | dd of=<DEV> status=progress bs=10M
```

Workaround, if image doesn't boot because of size issues. Select GRUB boot entry, press TAB to edit:

```console
<GRUB CMD LINE> ... mem=16000m
```

After booting from rescue medium (USB or ISO), work either from rescue system
directly or connect via SSH to it.

If there are problems with getting DHCP IP directly, as a workaround boot with
adding `noip` to kernel commandline, afterwards get IP via `dhclient`:

```console
<GRUB CMD LINE> ... noip
REAR# dhclient eth0
REAR# ip a
```

Connect via SSH, where resuce system has new host keys:

```console
$ ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no root@<REAR-IP>
```

Connect via SSH, where resuce system has unchanged host keys:

```console
$ ssh root@<REAR-IP>
```

Start recover:

```console
REAR# rear -v recover
```

Prepare backup restore and actually restore:

```console
REAR# source /root/source-prepare
REAR# cat << EOF > /root/.config/borg/keys/key
<BORGKEY>
EOF
REAR# cat << EOF > /etc/borgmatic/.passphrase
<BORGPASSPHRASE>
EOF
REAR# borgmatic info
REAR# borgmatic list
REAR# cd /mnt/local
REAR# mount -o remount,rw /dev/sdb1 /mnt/local/boot
REAR# borgmatic -v 1 extract --repository <REPO> --archive latest
REAR# exit
```

Verify restore, e.g. if LUKS setup is correctly done:

```console
REAR# cat /mnt/local/etc/crypttab
REAR# [lsinitrd /mnt/local/boot/initrd -f /etc/crypttab]
```

Reboot:

```console
REAR# reboot
```
