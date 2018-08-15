---
title: Virtualization
last-changed: <time>2018-08-15</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [openSUSE Virtualization Guide](https://doc.opensuse.org/documentation/leap/virtualization/html/book.virt/index.html) <time>2018-08-15</time>
* [SLES 12 Virtualization Guide](https://www.suse.com/documentation/sles-12/singlehtml/book_virt/book_virt.html) <time>2018-08-15</time>
* [KVM](https://www.linux-kvm.org/page/Main_Page) <time>2018-08-15</time>
* [libvirt](https://libvirt.org) <time>2018-08-15</time>
* [The Syslinux Project](http://www.syslinux.org) <time>2018-08-15</time>
* [openSUSE wiki: PXE boot installation](https://en.opensuse.org/SDB:PXE_boot_installation)  <time>2018-08-15</time>

## Terms

KVM
: Kernel Virtual Machine

SDS
: Software Defined Storage

SDN
: Software Defined Networking

## Virtualization

Paravirtualized vs. fullvirtualized

## Setup

``` sh
# egrep '(vmx|svm)' --color=always /proc/cpuinfo
# LANG=C lscpu | grep Virtualization
# zypper in -t pattern kvm_server
# zypper in libvirt qemu-kvm virt-manager
# lsmod | grep kvm
# ls -l /dev/kvm
```

``` sh
# usermod -a -G kvm <USER>
# usermod -a -G libvirt <USER>
# usermod -a -G qemu <USER>
```

``` text
# /etc/libvirt/libvirtd.conf

#listen_tcp = 1
unix_sock_group = "libvirt"
unix_sock_rw_perms = "0770"
auth_unix_rw = "none"
```

``` text
// /etc/polkit-1/rules.d/10-virt.rules

polkit.addRule(function(action, subject) {
  if (action.id == "org.libvirt.unix.manage" &&
      subject.local && subject.active && subject.isInGroup("libvirt")) {
      return polkit.Result.YES;
  }
});
```

``` sh
# systemctl enable libvirtd
# systemctl start libvirtd
```

``` text
# ~/.bashrc

export VIRSH_DEFAULT_CONNECT_URI='qemu:///system'
export LIBVIRT_DEFAULT_URI=$VIRSH_DEFAULT_CONNECT_URI
```

### Nested virtualization

Check for nested capability:

``` sh
# modinfo kvm_intel | grep -i nested
# modinfo kvm_amd | grep -i nested
```

``` text
# /etc/modprobe.d/99-kvm.conf

options kvm_intel nested=1
options kvm_amd nested=1
```

Check if nested is enabled:

``` sh
# cat /sys/module/kvm_intel/parameters/nested
# cat /sys/module/kvm_kvm/parameters/nested
# systool -m kvm_intel -v | grep -i nested
# systool -m kvm_amd -v | grep -i nested
```

For a guest vm (that is host for nested vm) in virt-manager under processor
copy host cpu configuration.

At least require vmx flag to be presented to guest.

``` text
<!-- <VM>.xml -->
<cpu mode='host-passthrough'/>
```

## libvirt / virsh

Connect to local libvirt host:

``` sh
# virsh -c 'qemu:///system'
```

If bash variable `VIRSH_DEFAULT_CONNECT_URI` is set:

``` sh
# virsh
```

Connect to a remote libvirt host:

``` sh
# virsh -c 'qemu+ssh://<USER>@<HOST>/system'
```

### Virtual machines

``` sh
# virsh start <VM>
# virsh shutdown <VM>
```

Undefine domain:

``` sh
# virsh undefine --remove-all-storage <VM>
```

### Storage pools

Autostart pool:

``` sh
# virsh pool-autostart [--disable] <POOL>
```

Destroy and undefine pool:

``` sh
# virsh pool-destroy <POOL>
# virsh pool-undefine <POOL>
```

### Volumes

By default volumes are stored under `/var/lib/libvirt/images`.

List volumes in pool:

``` sh
# virsh vol-list <POOL>
```

Create new volume:

``` sh
# virsh vol-create-as --pool <POOL> --name <VOL>.img --capacity 40G --format qcow2
```

Display info about volume:

``` sh
# virsh vol-info --pool <POOL> <VOL>.img
```

Delete volume:

``` sh
# virsh vol-delete --pool <POOL> <VOL>.img
```

Mount CD / DVD / iso into guest:

``` sh
host # virsh change-media <VM> hdc <CD>.iso
guest # mount ...
guest # umount ...
host # virsh change-media <VM> hdc
```

Which storage does a guest access now?

``` sh
# virsh domblklist <VM> --details
```

### Networks

Networks are stored by default under `/etc/libvirt/qemu/networks`.

Create networks:

``` sh
# vim <NET>.xml
# virsh net-define <NET>.xml
# virsh net-autostart <NET>
# virsh net-start <NET>
```

Delete networks:

``` sh
# virsh net-destroy <NET>
# virsh net-undefine <NET>
```

## qemu

### Images

Allocate space (not sparse):

``` sh
# fallocate -l <SIZE>G <IMAGE>
```

Create image:

``` sh
# qemu-img create -f qcow2 <DISK-IMAGE> 40G
# qemu-img info <DISK-IMAGE>
```

Create new base image from old base plus delta image:

``` sh
# qemu-img convert <DELTA-IMAGE> -O qcow2 <NEW-BASE-IMAGE>
```

Re-sparse images:

``` sh
# cp --sparse=always <IMAGE-ORIG> <IMAGE-SPARSE>
```

## Programs

* virt-manager (Virtual Machine Manager)
* virt-viewer
* vm-install

### kpartx

List partition mappings:

``` sh
# kpartx -l <IMAGE>
```

Add partition mappings:

``` sh
# kpartx -av <IMAGE>
```

Delete partition mappings:

``` sh
# kpartx -d <IMAGE>
```

### losetup

``` sh
# losetup -a
# losetup -d /dev/loop<XY>
```

### dmsetup

``` sh
# dmsetup
```

## Install VM

### Import existing disk image

``` sh
# virt-install -n <NAME> -r 512 -w network=default --disk <IMG>.img \
  --vnc --noautoconsole --import
```
