---
title: Patch
last-changed: <time>2018-11-05</time>
knowledgebase: true
categories: [Linux, Programming]
---
## Usage

Create patch:

``` sh
$ diff -u file file_patched > file.patch
```

Use patch:

``` sh
$ ls file
$ patch < file.patch
```
