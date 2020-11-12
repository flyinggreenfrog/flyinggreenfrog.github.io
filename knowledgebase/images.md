---
title: Images
last-changed: <time>2020-11-10</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [EXIF Orientation](http://www.impulseadventure.com/photo/exif-orientation.html) <time>2020-10-25</time>
* [MWG Tags](http://exiftool.org/TagNames/MWG.html) <time>2020-10-25</time>
* [Metadata](http://www.photometadata.org/meta-resources-field-guide-to-metadata) <time>2020-10-25</time>
* [Metadaten mit Exiftool](http://www.linux-community.de/Internal/Artikel/Print-Artikel/LinuxUser/2010/09/Metainformationen-bearbeiten-und-Bilder-organisieren-mit-Exiftool) <time>2020-10-25</time>
* [Digitale Fotosammlung](http://www.gerhard.fr/DAM/deutsch.php) <time>2020-10-25</time>
* [JPEG Bilder drehen](http://www.linux-community.de/Internal/Artikel/Print-Artikel/LinuxUser/2005/10/JPEG-Bilder-automatisch-umbenennen-und-verlustlos-bearbeiten) <time>2020-10-25</time>
* [Bad PreviewIFD directory error with exiftool](https://codeyarns.com/tech/2014-12-05-bad-previewifd-directory-error-with-exiftool.html) <time>2020-10-25</time>
* [ubuntuusers: ExifTool](https://wiki.ubuntuusers.de/ExifTool) <time>2020-10-25</time>
* [Simple Media Transfer Protocol File System](https://github.com/phatina/simple-mtpfs) <time>2020-10-25</time>
* [Thumbsup HTML galleries](https://thumbsup.github.io/docs) <time>2020-10-25</time>

## Terms

EXIF
: Exchangeable Image File Format

IPTC-IIM
: International Press Telecommunications Council - Information Interchange Model

XMP
: Extensible Metadata Platform

MWG
: Metadata Working Group

## Workflow

1. Mount (either sdcard directly or phone via e.g. `simple-mtpfs`).
2. Copy pictures to a temporary directory (e.g. with own script `syncdirs`).
3. Rotate (e.g. with own scripts `rotate+90` or `rotate-90`).
4. Delete bad pictures.
5. Correct date/time, if necessary.
6. Add EXIF metadata (e.g. with own script `addmetadata`).
7. Add EXIF tags, if needed.
8. Find duplicates (e.g. with `dupeguru`).
9. Rename and sort (e.g. with own script `sortimages`).
10. Create soft links for "albums" of chosen pictures (e.g. with NNN).
11. Use `git-annex`.
11. Create albums with thumbsup.

## Mount with simple-mtpfs

Installation:

```console
# zypper in simple-mtpfs
```

Show which devices are connected:

```console
$ simple-mtpfs -l
```

Make sure device is connected in transfer photos mode, if e.g. you get
following error:

```text
Could not retrieve device storage.
For android phones make sure the screen is unlocked.
```

If you get the following error with simple-mtpfs, device is likely already
mounted otherwise (e.g. with Gnome):

```text
LIBMTP PANIC: Trying to dump the error stack of a NULL device!
```

Mount:

```console
$ mkdir mnt
$ simple-mtpfs mnt
$ cd mnt
```

Unmount:

```console
$ cd
$ fusermount -u mnt
```

## Rotate

### Jpegtran

The tool `jpegtran` deletes EXIF data!

```console
# zypper in libjpeg-turbo
```

If rotation can't be done lossless, then abort:

```console
$ jpegtran -rotate 90 -perfect -output <ROTATED>.jpg <ORIGINAL>.jpg
```

Drop non-transformable blocks:
```console
$ jpegtran -rotate 90 -trim -output <ROTATED>.jpg <ORIGINAL>.jpg
```

### Exiftran

The tool `exiftran` doesn't delete EXIF data. Therefore use `exiftran` instead
of `jpegtran`.

```console
# zypper in exiftran
```

Options:

-i
: inplace editing

-9
: rotate clockwise

-2
: rotate counter clockwise

Rotate clockwise:

```console
$ exiftran -i -9 <IMAGE>.jpg
```

Rotate counter clockwise:

```console
$ exiftran -i -2 <IMAGE>.jpg
```

## Exiftool

```console
# zypper in exiftool
```

Show currently set information of an image:

```console
$ exiftool <IMG>.jpg
```

Options:

-m | -ignoreMinorErrors
: ignore minor errors

-P | -preserve
: preserve file modification date/time

-b | -binary
: extract binary data (e.g. for preview/thumbnail images)

-r | -recurse
: recursively process subdirectories

Get rid of 'Warning: Bad PreviewIFD directory':

```console
$ exiftool -m -P -overwrite_original -tagsfromfile @ *.jpg
```

If not there already, set createdate:

```console
$ exiftool -P -overwrite_original -mwg:'createdate=YYYY:MM:DD hh:mm:ss' <IMG>.jpg
```

Reset modifydate to createdate:

```console
$ exiftool -P -overwrite_original -'filemodifydate<createdate' *.jpg
```

Adjust all dates, e.g. if camera time was incorrect:

```console
$ exiftool -AllDates+='HH:MM' -overwrite_original <IMG>.jpg
```

MWG Tags:

* copyright
* creator
* rating
* description
* keywords

Add some EXIF metadata:

```console
$ exiftool -m -P -overwrite_original \
  -mwg:creator='<NAME>' \
  -mwg:'copyright<Copyright $createdate, <NAME> (<ADDRESS>), all rights reserved' \
  -d %Y *.jpg
```

Extract preview/thumbnail image:

```console
$ exiftool -b -PreviewImage <IMG>.jpg > <IMG>_preview.jpg
$ exiftool -b -ThumbnailPreviewImage <IMG>.jpg > <IMG>_preview.jpg
```

Add EXIF tags:

```console
$ exiftool -P -overwrite_original -mwg:keywords+='<TAG>' *.jpg
```

%%-c
: if multiple files add `-1`

%%+c
: if multiple files add `_1`

Sort pictures into subdirectories with filenames according to createdate:

```console
$ exiftool -P -r \
  -'Filename<DateTimeOriginal' \
  -d %Y-%m/%Y%m%d_%H%M%S%%-c.%%le *.jpg
```
