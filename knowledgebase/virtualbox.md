---
title: Virtualbox
last-changed: <time>2018-08-15</time>
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

``` sh
# zypper in virtualbox virtualbox-qt
```

Add to vboxusers group:

``` sh
# usermod -a -G vboxusers <USER>
```

``` sh
# systemctl enable vboxdrv
# systemctl start vboxdrv
```

### With DKMS

openSUSE:

``` sh
# zypper in dkms
```

CentOS:

``` sh
# yum install epel-release
# yum install dkms
```

``` sh
# /usr/lib/virtualbox/vboxdrv.sh setup
```

### Without DKMS

``` sh
# #rcvboxdrv setup
# /usr/lib/virtualbox/vboxdrv.sh setup
```

### Guest Additions

CentOS:

``` sh
guest# yum install bzip2 gcc cpp make kernel-devel-$(uname -r)
guest# export KERN_DIR=/usr/src/kernels/$(uname -r)
```

openSUSE/SLES:

``` sh
guest# uname -r
guest# zypper in kernel-default-devel = <VERSION>
guest# zypper in gcc cpp make
```

SLES:

``` sh
# vim /etc/modprobe.d/10-unsupported-modules.conf
allow_unsupported_modules 1
```

Ubuntu:

``` sh
guest# apt-get install gcc cpp make
```

Debian:

``` sh
guest# apt-get install build-essential module-assistant
guest# m-a prepare
```

* `/usr/share/virtualbox/VBoxGuestAdditions.iso`
* In VirtualBox select _Devices_, then _Insert Guest Additions CD image_.
* Use `dkms` or run `rcvboxadd setup` after every kernel update.

Mount guest additions and install them:

``` sh
guest# mount /dev/cdrom /mnt
guest# cd /mnt
guest# ./VBoxLinuxAdditions.run
```

Uninstall guest additions:

``` sh
guest# ./VBoxLinuxAdditions.run uninstall
```

## Usage

``` sh
$ VBoxManage list vms|runningvms
$ VBoxManage startvm <VM> [--type headless]
$ VBoxManage controlvm <VM> acpipowerbutton
```

Copy image:

``` sh
$ VBoxManage clonemedium disk <ORIG>.vdi <COPY>.vdi
```
