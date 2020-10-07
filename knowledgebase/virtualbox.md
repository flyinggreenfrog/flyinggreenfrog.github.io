---
title: Virtualbox
last-changed: <time>2020-10-07</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Virtualbox](https://www.virtualbox.org) <time>2018-08-15</time>

## Terms

DKMS
: Dynamic Kernel Module Support

## VirtualBox

Frontends:

* VirtualBox
* VBoxManage
* VBoxSDL
* VBoxHeadless

Kernel modules:

* vboxdrv
* vboxnetflt
* vboxnetadp

## Setup

```console
# zypper in virtualbox virtualbox-qt
```

Add to vboxusers group:

```console
# usermod -a -G vboxusers <USER>
```

```console
# systemctl enable vboxdrv
# systemctl start vboxdrv
```

### With DKMS

openSUSE:

```console
# zypper in dkms
```

CentOS:

```console
# yum install epel-release
# yum install dkms
```

```console
# /usr/lib/virtualbox/vboxdrv.sh setup
```

### Without DKMS

```console
# #rcvboxdrv setup
# /usr/lib/virtualbox/vboxdrv.sh setup
```

### Guest Additions

CentOS:

```console
guest# yum install bzip2 gcc cpp make kernel-devel-$(uname -r)
guest# export KERN_DIR=/usr/src/kernels/$(uname -r)
```

openSUSE/SLES:

```console
guest# uname -r
guest# zypper in kernel-default-devel = <VERSION>
guest# zypper in gcc cpp make
```

SLES:

```console
# vim /etc/modprobe.d/10-unsupported-modules.conf
allow_unsupported_modules 1
```

Ubuntu:

```console
guest# apt-get install gcc cpp make
```

Debian:

```console
guest# apt update -y && apt upgrade
guest# apt install dkms linux-headers-$(uname -r) build-essential
```

* Before (or in Windows):
  - In VirtualBox GUI select _Devices_, then _Insert Guest Additions CD image_.
  - Not there anymore: `/usr/share/virtualbox/VBoxGuestAdditions.iso`
* Now:
  - Instead download `https://download.virtualbox.org/virtualbox/6.0.10/VBoxGuestAdditions_6.0.10.iso`
  - Download it, when asked for automatical download.
* Use `dkms` or run `rcvboxadd setup` after every kernel update.

Mount guest additions and install them:

```console
guest# mount /dev/cdrom /mnt
guest# cd /mnt
guest# ./VBoxLinuxAdditions.run
```

Uninstall guest additions:

```console
guest# ./VBoxLinuxAdditions.run uninstall
```

With Virtualbox guest additions screen resolution will be adjusted
automatically, if you resize the window.

If not go to the display menu and select automatic adjustment of resolution.

## Usage

```console
$ VBoxManage list vms|runningvms
$ VBoxManage startvm <VM> [--type headless]
$ VBoxManage controlvm <VM> acpipowerbutton
```

Copy image:

```console
$ VBoxManage clonemedium disk <ORIG>.vdi <COPY>.vdi
```

List hdds:

```console
$ VBoxManage list hdds
```

Delete hdd:

```console
$ VBoxManage closemedium disk <UUID> --delete
```
