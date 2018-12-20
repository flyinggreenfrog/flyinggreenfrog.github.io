---
title: Storage
last-changed: <time>2018-12-20</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Festplattenstatus](https://wiki.ubuntuusers.de/Festplattenstatus) <time>2018-12-20</time>
* [Festplatten-Geschwindigkeitstest](https://wiki.ubuntuusers.de/Festplatten-Geschwindigkeitstest) <time>2018-12-20</time>
* [Linux I/O Performance Tests mit dd](https://www.thomas-krenn.com/de/wiki/Linux_I/O_Performance_Tests_mit_dd) <time>2018-12-20</time>
* [DD mit direct oder synchronized I/O](https://www.thomas-krenn.com/de/wiki/Dd_mit_direct_oder_synchronized_I/O) <time>2018-12-20</time>
* [Festplattengeschwindigkeit mit hdparm](http://www.pro-linux.de/kurztipps/2/1673/festplattengeschwindigkeit-mit-hdparm-vermessen.html) <time>2018-12-20</time>
* [Archlinux: RAID](https://wiki.archlinux.org/index.php/RAID) <time>2018-12-20</time>
* [Linux md RAID 10 disk layout](https://www.finnie.org/2017/04/01/linux-md-raid-10-disk-layout-updated) <time>2018-12-20</time>

## Terms

ATAPI
: Advanced Technology Attachment with Packet Interface

DMA
: Direct Memory Access

MD
: Multiple Devices

MDMA
: Multiword DMA

PIO
: Programmed Input Output

RAID
: Redundant Array of Independent Disks

SCSI
: Small Computer System Interface

SMART
: Self-Monitoring, Analysis and Reporting Technology

UDMA
: Ultra DMA

## Info

USB 1.1
: 1.5 or 12 Mbit/s

USB 2.0
: 480 Mbit/s (60 MByte/s)

USB 3.0
: 4000 Mbit/s (500 Mbyte/s)

SATA
: 1.5 Gbit/s

SATA 2
: 3 Gbit/s

SATA 3
: 6 Gbit/s

eSATA
: 3 Gbit/s

## Setup

### Ext3

The only journaling mode supported in RHEL is `data=ordered` (default).

Converting from ext2 to ext3:

``` sh
# tune2fs -j <DEV>
```

Reverting to ext2:

``` sh
# tune2fs -O ^has_journal <DEV>
```

## Speed Measurement

Activate / deactivate cache:

``` sh
# hdparm -W1 <DEV>
# hdparm -W0 <DEV>
```

Delete page cache:

``` sh
# echo 3 > /proc/sys/vm/drop_caches
```

### hdparm

``` sh
$ man 8 hdparm
```

``` sh
# hdparm -i <DEV>
# hdparm -I <DEV>
# hdparm -d|-d1|-d0 <DEV>
# hdparm -X <NR> <DEV> # PIO: 8,9,10,11,12 MDMA: 32,33,34 UDMA: 64,65,66,67,68,69
# hdparm -m16 <DEV> # Multi-Sector-I/O
```

``` sh
# hdparm -tT --direct [--offset <GB>] <DEV>
```

Options:

`-t`
: Timing device reads

`-T`
: Timing cache reads

`--direct`
: Use `O_DIRECT`, bypassing page cache

`--offset <GB>`
: Timings at other positions

### sdparm

``` sh
$ man 8 sdparm
```

``` sh
# sdparm <DEV>
# sdparm -a <DEV>
# sdparm --page=ca|co [-D|--defaults] [-S|--save] [-s|--set|--clear] <DEV>
# sdparm -C|--command eject|unlock <DEV>
```

### sysctl

``` sh
$ man 8 sysctl
$ man 5 sysctl.conf
```

``` text
# /etc/sysctl.conf

net.ipv4.ip_forward = 0
```

``` sh
# ls /proc/sys
# sysctl [-n] variable
# sysctl -w variable=value
# sysctl -p [<FILE>]
```

### dd

oflag options:

`direct`
: `O_DIRECT`, bypassing page cache

`dsync`
: `O_DSYNC`, synchronized I/O for data

`sync`
: `O_SYNC`, synchronized I/O for data and metadata

### Data transfer rate / throughput / Durchsatz

Write:

``` sh
# dd if=/dev/zero of=tempfile bs=1G count=1 oflag=direct
```

Read:

``` sh
# dd if=tempfile of=/dev/null bs=1G count=1 iflag=direct
```

### Latency / Latenz

Write:

``` sh
# dd if=/dev/zero of=tempfile bs=512 count=1000 oflag=direct
```

Read:

``` sh
# dd if=tempfile of=/dev/null bs=512 count=1000 iflag=direct
```

## Harddiskdrive check

### smartctl

``` sh
# update-smart-drivedb
```

``` sh
# smartctl -i <DEV>
```

`smartctl` options:

`-a`
: `--all`

`-i`
: `--info`

`-H`
: `--health`

## badblocks

``` sh
# badblocks -vso badblocks.txt <DEV>
```

`badblocks` options:

`-v`
: verbose

`-s`
: show progress

`[-n]`
: non destructive write test

`[-w]`
: destructive write test

`-o`
: output to file


## mdadm

GPT partition type: Linux RAID (A19D880F-05FC-4D3B-A006-743F0F84911E)

``` sh
# cfdisk -z <DEVICE>
# partprobe
```

``` sh
$ man 8 mdadm
$ man 5 mdadm.conf
```

``` text
# Modi
Assemble
Build
Create
Manage
Misc
Follow|Monitor
Grow
```

``` text
# /etc/mdadm.conf

# DEVICES <DEVICES>
DEVICES /dev/sd[bc][1]

# ARRAY <MD-DEVICE> devices=<DEVICE1>,<DEVICE2>
ARRAY /dev/md0 devices=/dev/sdb1,/dev/sdc1
```

``` sh
# mdadm --create <MD-DEVICE> --level=<RAIDLEVEL> --raid-devices=<NR> <DEVICES>
```

RAID10 with 4 disks:

``` sh
# mdadm --create --verbose --level=10 --metadata=1.2 --chunk=512K \
  --raid-devices=4 --layout=f2 /dev/md0 \
  /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
```

``` sh
# cat /proc/mdstat
# watch -n1 cat /proc/mdstat
# mdadm --detail <MD-DEVICE> | --scan
# mdadm --examine <DEVICE>
# mdadm --query <MD-DEVICE>|<DEVICE>
```

Remove harddisk:

``` sh
# mdadm --fail <MD-DEVICE> <DEVICE>
# mdadm --remove <MD-DEVICE> <DEVICE>
# mdadm --zero-superblock <DEVICE>
```

``` sh
# mdadm --stop <MD-DEVICE>
# mdadm --assemble <MD-DEVICE>
# mdadm --assemble --scan
# mdadm --zero-superblock <DEVICE>
```
