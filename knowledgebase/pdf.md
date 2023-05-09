---
title: PDF
last-changed: 2023-05-09.
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

## mupdf

Split a 2-pages per page PDF into a single-pages per page PDF:

```sh
$ mutool poster -x 2 <INPUT>.pdf <OUTPUT>.pdf
```

## qpdf

```sh
# zypper in qpdf
```

Remove password (even empty password):

```sh
$ qpdf --password='<PASSWORD>' --decrypt <FILE>.pdf <NEWFILE>.pdf
```

## pdfinfo

Get infos, e.g. papersize:

```sh
$ pdfinfo <FILE>.pdf
```

## pdfjam

```sh
# zypper in --no-recommends pdfjam
```

Resize to A4 paper:

```sh
$ pdfjam --outfile <OUTPUT>.pdf --paper a4paper <INPUT>.pdf
```

Mount 2 pdfs on one A4 page:

```sh
$ pdfjam --nup 1x2 --no-landscape --outfile <OUTPUT>.pdf <INPUT-1>.pdf <INPUT-2>.pdf
```

## convert

Convert images colorspace to grayscale:

```sh
$ convert <IMAGE>.jpg -set colorspace RGB -colorspace gray <IMAGE-GRAY>.jpg
```

Convert photos to pdf:

```sh
$ convert <IMAGE>.jpg <IMAGE>.pdf
```


## xournal

```sh
# zypper in xournal
```

With xournal you can fill a pdf form (graphically as overlay picture).
