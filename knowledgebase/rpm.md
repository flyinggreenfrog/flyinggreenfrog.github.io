---
title: RPM
last-changed: <time>2021-01-30</time>
knowledgebase: true
categories: [Linux]
---
## Terms

RPM
: RPM Package Manager (formerly RedHat Package Manager)

## Usage

Find package that a installed file belongs to:

```console
# rpm -qf <FILEPATH>
```

Verify installed package:

`S`
: file Size differs

`M`
: Mode differs (includes permissions and file type)

`5`
: digest (formerly MD5 sum) differs

`D`
: Device major/minor number mismatch

`L`
: readLink(2) path mismatch

`U`
: User ownership differs

`G`
: Group ownership differs

`T`
: mTime differs

`P`
: caPabilities differ

```console
# rpm -V <PACKAGE>
```
