---
title: Patch
last-changed: <time>2021-02-03</time>
knowledgebase: true
categories: [Linux, Programming]
---
## Usage

Create patch:

```console
$ cp <DIR>/{<FILE>,<NEW>}
$ vim <DIR>/<NEW>
$ diff -u <DIR>/{<FILE>,<NEW>} > patch.txt
$ rm <DIR>/<NEW>
```

`-p0`
: use entire filename unmodified, don't strip anything

`-N | --forward`
: don't try to reverse-apply patch

`-q`
: work silently, unless error occurs

`--dry-run`
: print result without actually changing anything

Use patch:

```console
$ ls <DIR>/<FILE>
$ patch -p0 < patch.txt

```

Apply patch only, if file wasn't patched yet:

```console
$ patch -p0 -N --dry-run -q 2> /dev/null < patch.txt && patch -p0 -N < patch.txt || true
```
