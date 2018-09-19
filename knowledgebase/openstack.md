---
title: Openstack
last-changed: <time>2018-09-19</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Openstack Documentation](https://docs.openstack.org) <time>2018-08-09</time>
* [Openstack Operations Guide](https://docs.openstack.org/operations-guide) <time>2018-08-23</time>
* [Openstack Project Navigator](https://www.openstack.org/software/project-navigator/openstack-components#main-services) <time>2018-08-15</time>

## Terms

HOT
: Heat Orchestration Template

## Projects

Core:

* Nova
* Neutron
* Swift
* Glance
* Cinder
* Keystone

Others:

* Ceilometer
* Trove
* Sahara
* Ironic
* Zaqar
* Manila
* Designate
* Barbican
* Magnum
* Murano
* Congress

## Network

Agents:

* DHCP (network)
* L3 (network, compute)
* Metadata (network, compute)
* LbaaS (network)
* OVS (network, compute)

Flooding vs. L2 Population

Neutron: Legacy vs. DVR

## Usage

List VMs on hypervisor:

``` sh
$ openstack server list [--long] --all-projects --host <HOST>
$ nova list --all-tenants --host <HOST>
```

List VMs in Error state:

``` sh
$ openstack server list --all-projects --limit -1 --status ERROR -f value -c ID
```

Create VM:

``` sh
$ openstack flavor list
$ nova flavor-list

$ openstack image list
$ glance image-list

$ neutron net-list --tenant-id <PROJECT>

$ openstack server create --flavor <FLAVOR> --image <IMAGE> \
  --nic net-id=<NET> [--availability-zone <AZ>:<HOST>] <VM>
$ nova boot --flavor <FLAVOR> --image <IMAGE> --nic net-id=<NET> \
  [--availability-zone <AZ>:<HOST>] <VM>
```

Live migrate VM:

``` sh
$ openstack server migrate <VM> --live <TARGET-HOST>
$ nova live-migration <VM> [<TARGET-HOST>]
```
