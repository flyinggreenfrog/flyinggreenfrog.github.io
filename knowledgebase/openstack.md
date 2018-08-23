---
title: Openstack
last-changed: <time>2018-08-23</time>
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

## Usage

List VMs in Error state:

``` sh
$ openstack server list --all-projects --limit -1 --status ERROR -f value -c ID
```
