---
title: LUKS
last-changed: <time>2018-12-21</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Arch: Disk encryption](https://wiki.archlinux.org/index.php/Disk_encryption) <time>2018-12-21</time>
* [Arch: dm-crypt/Encrypting an entire system](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system) <time>2018-12-21</time>
* [Full disk encryption with LUKS (including /boot)](http://www.pavelkogan.com/2014/05/23/luks-full-disk-encryption) <time>2018-12-21</time>
* [Ubuntuusers LUKS](https://wiki.ubuntuusers.de/LUKS) <time>2018-12-21</time>
* [DM-Crypt und Cryptsetup-LUKS](http://www.linux-magazin.de/Ausgaben/2005/08/Geheime-Niederschrift) <time>2018-12-21</time>
* [Automatically unlock LUKS encrypted drives with a keyfile](https://www.howtoforge.com/automatically-unlock-luks-encrypted-drives-with-a-keyfile) <time>2018-12-21</time>

## Terms

AES
: Advanced Encryption Standard

CBC
: Cipher Block Chaining

DES
: Data Encryption Standard

DM
: Device Mapper

ECB
: Electronic Code Block

ESSIV
: Encrypted Salt-Sector Initialization Vector

LRW
: Liskov Rivest Wagner

LUKS
: Linux Unified Key Setup

PBKDF2
: Password-Based Key Derivation Function 2

SHA
: Secure Hash Algorithm

## Setup

``` sh
# modprobe loop
# modprobe dm_crypt
# zypper in cryptsetup
```

## Usage

Overwrite beginning:

``` sh
# dd if=/dev/urandom of=<DEV> bs=1M count=10
```

Help shows compiled-in defaults for cipher, key-size and hash:

``` sh
# cryptsetup --help
[...]
Default compiled-in device cipher parameters:
  loop-AES: aes, Key 256 bits
  plain: aes-cbc-essiv:sha256, Key: 256 bits, Password hashing: ripemd160
  LUKS1: aes-xts-plain64, Key: 256 bits, LUKS header hashing: sha1, RNG: /dev/urandom
```

Encrypt and format device:

``` sh
# cryptsetup --verify-passphrase --verbose \
  --cipher aes-xts-plain64 --key-size 512 --hash sha512 \
  luksFormat <DEV>
# cryptsetup open <DEV> <NAME>
# # Use /dev/mapper/<NAME> either LVM or format directly
# cryptsetup close <NAME>
```

Use device:

``` sh
# cryptsetup open <DEV> <NAME>
# udiskctl mount -b /dev/mapper/<NAME>
# udiskctl unmount -b /dev/mapper/<NAME>
# crypsetup close <NAME>
```

Info about crypto device:

``` sh
# dmsetup info <NAME>
# cryptsetup status <NAME>
```

## Header

Header information:

``` sh
# cryptsetup luksDump <DEV>
```

Backup header information of LUKS device:

``` sh
# cryptsetup luksHeaderBackup <DEV> --header-backup-file <BACKUP>
```

Restore header information of LUKS device:

``` sh
# cryptsetup luksHeaderRestore <DEV> --header-backup-file <BACKUP>
```

Delete header, that means securely delete the data, if no header backup.

Wipe LUKS header, to be on the safe side wipe 10 MiB:

``` sh
# dd if=/dev/urandom of=<DEV> bs=1M count=10
```

## Passwords

With LUKS you can use multiple passwords, there are up to 8 slots.

Add key:

``` sh
# cryptsetup --verify-passphrase [--key-slot <NR>] luksAddKey <DEV>
```

Remove key:

``` sh
# cryptsetup luksRemoveKey <DEV>
```

``` sh
# cryptsetup luksKillSlot <DEV> <SLOT>
```

Test passphrase or keyfile:

``` sh
# cryptsetup open --test-passphrase <DEV> [--key-file <KEYFILE>]
```

## Keyfiles

Create keyfile:

``` sh
# dd bs=512 count=4 if=/dev/urandom | base64 > /etc/keyfile
# chown root. /etc/keyfile
# chmod 400 /etc/keyfile
```

Add keyfile:

``` sh
# cryptsetup luksAddKey <DEV> /etc/keyfile
```

Use keyfile:

``` sh
# cryptsetup open <DEV> <NAME> --key-file /etc/keyfile
```

## Container

Create container file:

``` sh
$ dd if=/dev/urandom of=<CONTAINER> bs=1G count=1
```

Then use `<CONTAINER>` as device.

Info about loop devices:

``` sh
# losetup -l
```

## Crypttab

``` text
# /etc/crypttab
<NAME> <DEVICE> <KEYFILE> luks
```
