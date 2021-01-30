---
title: Packaging
last-changed: <time>2021-01-30</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [RPM Packaging Guide](https://rpm-packaging-guide.github.io) <time>2021-01-30</time>
* [RPM Packaging Guide](http://rpm-guide.readthedocs.io/en/latest/) <time>2021-01-30</time>
* [Maximum RPM](http://www.rpm.org/max-rpm) <time>2021-01-30</time>
* [RPM Macro syntax](http://rpm.org/user_doc/macros.html) <time>2021-01-30</time>
* [openSUSE: Specfile guidelines](https://en.opensuse.org/openSUSE:Specfile_guidelines) <time>2021-01-30</time>
* [openSUSE: Packaging Conventions RPM Macros](https://en.opensuse.org/openSUSE:Packaging_Conventions_RPM_Macros) <time>2021-01-30</time>

## Description

Where to place files:

* `/usr/bin/`
* `/usr/share/<PROG>/lib/`

## Packaging

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

## Specfiles

### `%prep` section

`-q`
: quiet

`-n <DIR>`
: set name of build directory

```text
%prep
%setup -q -n
```
