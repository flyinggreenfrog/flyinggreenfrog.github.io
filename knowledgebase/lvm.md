---
title: LVM
last-changed: <time>2018-11-09</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Arch: LVM](https://wiki.archlinux.org/index.php/LVM) <time>2018-07-10</time>
* [LVM, Demystified](http://www.linuxjournal.com/content/lvm-demystified) <time>2018-07-10</time>

## Terms

LVM
: Logical Volume Manager

PV
: Physical Volume

VG
: Volume Group

LV
: Logical Volume

PE
: Physical Extent

## Setup

Configuration:

``` sh
$ man 8 lvm
$ man 5 lvm.conf
```

### `/etc/lvm/lvm.conf`

``` text
devices {
  dir = "/dev"
  scan = [ "/dev" ]
}
```

With LVM devices or partitions can be used.

Erase partion table:

``` sh
# dd if=/dev/zero of=<DEV> bs=512 count=1
```

Partition-ID: `8E (Linux LVM)`

``` sh
# cfdisk <DEV>
# partprobe
```

Use partition of type Linux LVM 0x8e (MBR), Linux LVM (GPT) or complete device:

``` sh
# lvmdiskscan
# pvcreate <PARTITION>|<DEV>
# vgcreate <VG-NAME> <PARTITION>|<DEV>
# lvcreate -L <SIZE>G -n <LV-NAME> <VG-NAME>
```

``` sh
# lvcreate -l 100%FREE -n <LV-NAME> <VG-NAME>
```

## Administration

Get infos:

``` sh
# pvs | pvdisplay | pvscan
# vgs | vgdisplay | vgscan
# lvs | lvdisplay | lvscan
```

Extend LV (live grow):

``` sh
# lvextend -L +<SIZE>G /dev/mapper/<VG-NAME>-<LV-NAME>
# lvextend -l +100%FREE /dev/mapper/<VG-NAME>-<LV-NAME>
```

Remove LV:

``` sh
# lvchange -an /dev/mapper/<VG-NAME>-<LV-NAME>
# lvremove /dev/mapper/<VG-NAME>-<LV-NAME>
```

Additional commands:

* `pvmove`
* `vgreduce | lvreduce`
* `vgrename | lvrename`
* `vgexport | vgimport`
* `vgmerge | vgsplit`

## Snapshots

Ensure enough space available for snapshot:

``` sh
# vgs
```

Create snapshot:

``` sh
# lvcreate -l 100%FREE -s -n snap /dev/mapper/<VG-NAME>-<LV-NAME>
# mount /dev/mapper/<VG-NAME>-snap /mnt
```

Then use it, e.g. make backup from '/mnt'.

Delete snapshot:

``` sh
# umount /mnt
# lvremove /dev/mapper/<VG-NAME>-snap
Do you really want to remove active logical volume snap? [y/n]: y
  Logical volume "snap" successfully removed
```
