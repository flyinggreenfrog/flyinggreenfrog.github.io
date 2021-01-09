---
title: Filesystems
last-changed: <time>2021-01-09</time>
knowledgebase: true
categories: [Linux]
---
## Fileystems

`lsblk` options:

`-f --fs`
: Output info about filesystems

Check, which devices/filesystems are available:

``` sh
# lsblk -f
```

Low level infos:

``` sh
# blkid
# blkid -L <LABEL>
```

## File system check

``` sh
# fsck -v <PARTITION>
```

Force check, even when it seems clean:

``` sh
# fsck -fv <PARTITION>
```

Additionally badblocks read test:

``` sh
# fsck -fcv <PARTITION>
```

Additionally badblocks non-destructive read write test:

``` sh
# fsck -fccv <PARTITION>
```

## Ext2 / Ext3 / Ext4

``` sh
# mkfs.ext4 -L <LABEL> -m 0 <PARTITION>
```

Resize ext2/ext3/ext4 fileystem:

``` sh
# resize2fs <PARTITION>
```

Set reserved space to 0:

``` sh
# tune2fs -m 0 <PARTITION>
```

Check reserved space:

``` sh
# tune2fs -l <PARTITION> | grep 'Reserved block count'
```

Change label:

``` sh
# tune2fs -L <LABEL> <PARTITION>
```

## Vfat

``` sh
# mkfs.vfat -n <LABEL> <PARTITION>
```

## NTFS

``` sh
# mkfs.ntfs -L <LABEL> <PARTITION>
```
