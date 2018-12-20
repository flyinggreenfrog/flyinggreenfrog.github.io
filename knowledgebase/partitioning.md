---
title: Partitioning
last-changed: <time>2018-12-20</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Archlinux: Partitioning](https://wiki.archlinux.org/index.php/Partitioning) <time>2018-12-20</time>
* [Archlinux: fdisk](https://wiki.archlinux.org/index.php/Fdisk) <time>2018-12-20</time>
* [Secrets of the GPT](https://developer.apple.com/library/mac/technotes/tn2166/_index.html) <time>2018-12-20</time>
* [Partition Alignment](https://www.thomas-krenn.com/de/wiki/Partition_Alignment#Linux) <time>2018-12-20</time>
* [Discoverable Partitions](https://www.freedesktop.org/wiki/Specifications/DiscoverablePartitionsSpec) <time>2018-12-20</time>

## Terms

GPT
: GUID Partition Table

GUID
: Globally Unique Identfifier

LBA
: Logical Block Addressing

MBR
: Master Boot Record

MPT
: Master Partition Table

## MPT

The MBR is the 1st sector (512 byte) at the beginning of a disk.

MBR uses 512 byte sectors and the maximal size of a partition is 2 TiB.

MBR:

* 0: Bootstrap code area (446 bytes)
* 446: partition table (4x 16 bytes = 64 bytes) (MPT)
* 510: bootsector signature: 55 AA (2 bytes)

Erase MBR, be careful, since it means dataloss:

``` sh
# dd if=/dev/zero of=<DEVICE> bs=512 count=1
```

Erase all signatures from a device:

``` sh
# wipefs --all --force <PARTITION>
# wipefs --all --force <DEVICE>
```

### Boot flag

A Boot flag set appears as `0x80` in the partition record, unset as `0x00`. It
was needed by old bootloaders (GRUB doesn't need it), but some BIOS may need a
bootable partition to recognize the disk as valid boot device.

## GPT

GPT uses 512 byte sectors with 64bit LBA.

GPT:

* Blocks 0 to 33: Primary GPT (Protective MBR, Header, Partitions)
* Blocks #LBA-33 to #LBA-1: Secondary GPT (Partitions, Header)

## Blockdevices

List blockdevices with filesystem information:

``` sh
# lsblk -f
```

Show blockdevices attributes:

``` sh
# blkid
```

## Partprobe

Ask kernel to reread partition tables:

``` sh
# partprobe
```

Which partition tables and partitions are there?

``` sh
# partprobe -s
```

## fdisk

Now fdisk supports GPT.

List partitions:

``` sh
# fdisk -l <DEV>
```

## cfdisk

Since version 2.25 cfdisk supports GPT.

`-z`
: start with in-memory zeroed partition table

Partitioning:

``` sh
# cfdisk [-z] <DEVICE>
```

MBR partition types:

* 82 Linux swap / Solaris
* 83 Linux
* 8e Linux LVM

GPT partition types:

* BIOS boot (21686148-6449-6E6F-744E-656564454649)
* Linux swap (0657FD6D-A4AB-43C4-84E5-0933C84B4F4F)
* Linux filesystem (0FC63DAF-8483-4772-8E79-3D69D8477DE4)
* Linux LVM (E6D6D379-F507-44C2-A23C-238F2A3DF928)
* Linux RAID (A19D880F-05FC-4D3B-A006-743F0F84911E)

## sfdisk

Since version 2.26 sfdisk supports GPT.

Save description of partition table:

``` sh
# sfdisk -d|--dump <DEVICE> > <TABLE>.dump
```

Restore partition table:

``` sh
# sfdisk <DEVICE> < <TABLE>.dump
```

Full (binary) backup of partition table to some files:

``` sh
# sfdisk -b|--backup <DEVICE>
sfdisk-<DEVICE>-<OFFSET>.bak
```

Restore partition table:

``` sh
# dd if=sfdisk-<DEVICE>-<OFFSET>.bak of=<DEVICE> seek=$((<OFFSET>)) bs=1 conv=notrunc
```

## gdisk, cgdisk, sgdisk

If fdisk, cfdisk and sfdisk are only available in older versions, you may use
gdisk, cgdisk and sgdisk instead.
