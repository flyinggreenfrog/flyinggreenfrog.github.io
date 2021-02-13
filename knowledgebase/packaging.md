---
title: Packaging
last-changed: <time>2021-02-13</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [RPM Packaging Guide](https://rpm-packaging-guide.github.io) <time>2021-02-13</time>
* [RPM Packaging Guide](http://rpm-guide.readthedocs.io/en/latest/) <time>2021-02-13</time>
* [Maximum RPM](http://www.rpm.org/max-rpm) <time>2021-02-13</time>
* [RPM Macro syntax](http://rpm.org/user_doc/macros.html) <time>2021-02-13</time>
* [Filesystem Hierarchy Standard (FHS)](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html) <time>2021-02-13</time>
* [openSUSE: Portal:Packaging](https://en.opensuse.org/Portal:Packaging) <time>2021-02-13</time>
* [openSUSE: Packaging guidelines](https://en.opensuse.org/openSUSE:Packaging_guidelines) <time>2021-02-13</time>
* [openSUSE: Specfile guidelines](https://en.opensuse.org/openSUSE:Specfile_guidelines) <time>2021-02-13</time>
* [openSUSE: Packaging Conventions RPM Macros](https://en.opensuse.org/openSUSE:Packaging_Conventions_RPM_Macros) <time>2021-02-13</time>
* [openSUSE: Build Service Tutorial](https://en.opensuse.org/openSUSE:Build_Service_Tutorial) <time>2021-02-13</time>
* [openSUSE: OSC](https://en.opensuse.org/openSUSE:OSC) <time>2021-02-13</time>
* [openSUSE: Build Service Tips and Tricks](https://en.opensuse.org/openSUSE:Build_Service_Tips_and_Tricks) <time>2021-02-13</time>
* [openSUSE: Build Service Concept project linking](https://en.opensuse.org/openSUSE:Build_Service_Concept_project_linking) <time>2021-02-13</time>

## Terms

FHS
: Filesystem Hierarchy Standard

## Description

Where to place files according to FHS:

* `/usr/bin/`
* `/usr/share/<PROG>/lib/`

## RPM

```console
# zypper in rpm-build rpm-devel rpmlint rpmdevtools
```

```console
$ rpmdev-setuptree
$ tree ~/rpmbuild
$ rpmdev-newspec hello-world
$ vim hello-world.spec
```

Build binary and source packages (keeping SOURCES/BUILD content):

```console
$ rpmbuild -ba hello-world.spec
```

Build source package first, then rebuild binary package from source package
(deleting SOURCES/BUILD content):

```console
$ rpmbuild -bs hello-world.spec
$ rpmbuild --rebuild ~/rpmbuild/SRPMS/hello-world-<VERSION>-<RELEASE>.src.rpm
```

Check content of rpm package:

```console
$ rpm -qlp <PKG>.rpm
```

Unpack rpm to current directory:

```console
$ rpm2cpio <PKG>.rpm | cpio -idmv
```

### Specfiles

`-q`
: quiet

`-n <DIR>`
: set name of build directory

```text
%prep
%setup -q -n
```

## OBS

* projects
* packages
* repositories

* Requires
* BuildRequires

* Add deps to repo
* Layering deps to repo (editing meta data)
* Aggregate (Link with modifying)

### Specfiles

Irrelevant in obs:

```text
Release: 1%{?dist}
```

### osc

Install:

```console
# zypper in osc obs-service-download_files
```

Usage:

```console
$ osc -A <APIALIAS> ...
```

`~/.oscrc` or `~/.config/osc/oscrc`:

```text
[general]
apiurl = <API>
plaintext_passwd = 0
su-wrapper = sudo

# if <API> section is deleted, user and passx will be readded
[<API>]
user = <LOGIN>
aliases = <APIALIAS>
#pass = <PASSWORD>
passx = <HASHED_PASSWORD>
```

Checkout existing projects:

```console
$ osc co <PROJECT> [<PACKAGE>]
```

Checkout `<PACKAGE>` inside `<PROJECT>`:

```console
$ osc co <PACKAGE>
```

Branch project, to make changes at sources:

```console
$ osc branch <PROJECT> <PACKAGE>
```

Copy packages (e.g. different build service):

```console
$ osc -A <APIALIAS> copypac -t <TO-APIALIAS> <SOURCEPRJ> <SOURCEPKG> <DESTPRJ> [<DESTPKG>]
```

View projects / packages:

```console
$ osc ls /
$ osc ls <PROJECT> [<PACKAGE>]
$ Binärpakete: osc ls -b
```

View working copy:

```console
$ osc info <PATH>
```

```console
$ osc log
```

Update working copy:

```console
$ osc up
```

Add files:

```console
$ osc add <FILE>
```

Changelog:

```
$ touch <PACKAGE>.changes
$ osc vc
```

`<PACKAGE>.changes`:

```text
* 2021-02-13 update
```

or

```text
Sat Feb 13 20:00:00 UTC 2021 - mail@example.org
- update
  * comments
```

```console
$ osc add <PACKAGE>.changes
```

Commit changes:

```console
$ osc ci
```

List repositories:

```console
$ osc repos
```

Trigger build manual (normally not necessary, because done automatically):

```console
$ osc rebuildpac [<PROJECT> [<PACKAGE> [<REPOSITORY> [<ARCH>]]]]
```

Build locally:

```console
$ osc build <REPOSITORY> <ARCH> <SPEC-FILE>
```

Status:

```console
$ osc st
```

Results:

```console
$ osc r
```

Project results:

```console
$ osc pr
```

Buildlog:

```console
$ osc bl|buildlog <REPOSITORY> <ARCH>
$ osc blt|buildlogtail <REPOSITORY> <ARCH>
```

Metainformation:

```console
$ osc meta prj|prjconf|pkg
```

Create new project:

```console
$ osc meta prj -e <PROJECT>
```

Create new package:

```console
$ osc meta pkg -e <PROJECT> <PACKAGE>
```

Repo:

```console
$ osc meta prj -e <PROJECT>
$ osc meta prjconf <PROJECT>
```

User:

```console
$ osc meta user <USER>
```

Delete package:

```console
$ cd <PROJECT>
$ osc delete <PACKAGE>
```
