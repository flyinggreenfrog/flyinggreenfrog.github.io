---
title: molecule
last-changed: <time>2020-11-12</time>
knowledgebase: true
categories: [Linux, Systemsmanagement]
---
## Links

* [Molecule](https://molecule.readthedocs.io/en/latest/) <time>2020-11-12</time>
* [Github: Molecule](https://github.com/ansible-community/molecule) <time>2020-11-12</time>
* [Github: Molecule Vagrant Plugin](https://github.com/ansible-community/molecule-vagrant) <time>2020-11-12</time>
* [Github: Molecule Goss Plugin](https://github.com/ansible-community/molecule-goss) <time>2020-11-12</time>
* [Developing and Testing Ansible Roles - Part 1](https://www.ansible.com/blog/developing-and-testing-ansible-roles-with-molecule-and-podman-part-1) <time>2020-11-12</time>
* [Developing and Testing Ansible Roles - Part 2](https://www.ansible.com/blog/developing-and-testing-ansible-roles-with-molecule-and-podman-part-2) <time>2020-11-12</time>
* [Ansible Testing Using Molecule with Ansible as Verifier](https://www.adictosaltrabajo.com/2020/05/08/ansible-testing-using-molecule-with-ansible-as-verifier/) <time>2020-11-12</time>

## Setup

```console
$ pip3 install --user molecule[lint]==3.0.8 molecule-vagrant==0.3
```

## Usage

### Libvirt Remote

`molecule.yml`

```text
dependency:
  name: 'galaxy'
driver:
  name: 'vagrant'
  provider:
    name: 'libvirt'
  ssh_connection_options:
    # Needed for Prepare, Converge, Verify and Login
    - "-o ProxyCommand='ssh -W %h:%p <USER>@<LIBVIRT-HOST>'"
    # alternative
    # - '-o ProxyJump=<USER>@<LIBVIRT-HOST>'
    # together with following molecule driver base default values
    # since molecule doesn't merge options with defaults
    - '-o UserKnownHostsFile=/dev/null'
    - '-o ControlMaster=auto'
    - '-o ControlPersist=60s'
    - '-o ForwardX11=no'
    - '-o LogLevel=ERROR'
    - '-o IdentitiesOnly=yes'
    - '-o StrictHostKeyChecking=no'
lint: |
  set -e
  yamllint .
  ansible-lint
platforms:
  # https://app.vagrantup.com/boxes/search
  - name: 'ansible-role-<ROLE>-<OS>'
    box: '<BOX>'
    box_version: '<BOX-VERSION>'
    # https://github.com/vagrant-libvirt/vagrant-libvirt#provider-options
    provider_options:
      # Needed for Create
      connect_via_ssh: true
      # '"Double quoting"' is necessary for correct handling of
      # variables/strings in ~/.cache/molecule/ansible-role-<ROLE>/<SCENARIO>/vagrant.yml
      username: '"<USER>"'
      host: '"<LIBVIRT-HOST>"'
    # Needed for Create (more than one platform)
    provider_override_args:
      - "ssh.proxy_command = 'ssh -W %h:%p <USER>@<LIBVIRT-HOST>'"
provisioner:
  name: 'ansible'
verifier:
  name: 'ansible'
```
