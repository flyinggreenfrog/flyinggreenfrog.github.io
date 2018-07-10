---
title: Delete
last-changed: <time>2018-07-10</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Heise: Sicheres Löschen](https://heise.de/-198816[Heise: Sicheres Löschen) <time>2018-07-10</time>

## Securely delete

According to research one time overwriting with zeroes is enough.

## shred

Options:

`-v`
: verbose

`-z`
: add final overwrite with zeros

`-n <N>`
: overwrite `<N>` times

Delete one time with zeros:

``` sh
# shred -vzn 0 <DEV>
```

## dd

Delete with zeros:

``` sh
# dd if=/dev/zero of=<DEV> conv=noerror,notrunc,sync bs=10M
```

Show progress (coreutils >= 8.24):

``` sh
# dd if=/dev/zero of=<DEV> status=progress conv=noerror,notrunc,sync bs=10M
```

Show progress (coreutils < 8.24):

``` sh
# dd if=/dev/zero of=<DEV> conv=noerror,notrunc,sync bs=10M & PID=$!
# while true; do kill -USR1 $PID && sleep 1 && clear; done
```

## Securely delete device

Create random password file:

``` sh
# pwgen 8 1 > keyfile
```

Setup luks device and open it:

``` sh
# cryptsetup luksFormat <DEV> keyfile
# cryptsetup open <DEV> wipe --key-file keyfile
```

Delete one time with zeros:

``` sh
# shred -vzn 0 /dev/mapper/wipe
# sync
```

Close luks device:

``` sh
# cryptsetup close wipe
```

Securely delete password file:

``` sh
# shred -vu keyfile
```

Wipe LUKS header (to be on the safe side wipe 10 MiB):

``` sh
# dd if=/dev/urandom of=<DEV> bs=10M count=1
```
