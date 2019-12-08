---
title: PDF
last-changed: <time>2019-12-08</time>
knowledgebase: true
categories: [Linux]
---
## Terms

PDF
: Portable Document Format

## pdftk

Burst:

```sh
$ pdftk <INPUT>.pdf burst output <OUTPUT>_%02d.pdf
```

Concat:

```sh
$ pdftk <INPUT>.pdf cat <FROM-TO> output <OUTPUT>.pdf
$ pdftk A=<INPUT1>.pdf B=<INPUT2> cat A<FROM-TO> B<FROM-TO> output <OUTPUT>.pdf
```

Rotate:

```sh
$ pdftk <INPUT>.pdf cat 1-endeast output <OUTPUT>.pdf
```

## qpdf

```sh
# zypper in qpdf
```

Remove password (even empty password):

```sh
$ qpdf --password='<PASSWORD>' --decrypt <FILE>.pdf <NEWFILE>.pdf
```

## pdfjam

Resize to A4 paper:

```sh
$ pdfjam --outfile <OUTPUT>.pdf --paper a4paper <INPUT>.pdf
```

## xournal

```sh
# zypper in xournal
```

With xournal you can fill a pdf form (graphically as overlay picture).
