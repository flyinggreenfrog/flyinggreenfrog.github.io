---
title: Images
last-changed: <time>2018-06-30</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [EXIF Orientation](http://www.impulseadventure.com/photo/exif-orientation.html) <time>2018-06-30</time>
* [MWG Tags](http://www.sno.phy.queensu.ca/~phil/exiftool/TagNames/MWG.html) <time>2018-06-30</time>
* [Metadata](http://www.photometadata.org/meta-resources-field-guide-to-metadata) <time>2018-06-30</time>
* [Metadaten mit Exiftool](http://www.linux-community.de/Internal/Artikel/Print-Artikel/LinuxUser/2010/09/Metainformationen-bearbeiten-und-Bilder-organisieren-mit-Exiftool) <time>2018-06-30</time>
* [Digitale Fotosammlung](http://www.gerhard.fr/DAM/deutsch.php) <time>2018-06-30</time>
* [JPEG Bilder drehen](http://www.linux-community.de/Internal/Artikel/Print-Artikel/LinuxUser/2005/10/JPEG-Bilder-automatisch-umbenennen-und-verlustlos-bearbeiten) <time>2018-06-30</time>

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

1. Copy from sdcard to a temporary directory.
2. Rotate (e.g. with `thunar`).
3. Delete bad pictures.
4. Correct date/time, if necessary.
5. Add metadata (e.g. with own script `addmetadata`).
6. Add tags.
7. Find duplicates (e.g. with `dupeguru`).
8. Rename and sort (e.g. with own script `sortimages`).

## Rotate

### Jpegtran

The tool `jpegtran` deletes EXIF data!

``` sh
# zypper in libjpeg-turbo
```

If rotation can't be done lossless, then abort:

``` sh
$ jpegtran -rotate 90 -perfect -output <ROTATED>.jpg <ORIGINAL>.jpg
```

Drop non-transformable blocks:
``` sh
$ jpegtran -rotate 90 -trim -output <ROTATED>.jpg <ORIGINAL>.jpg
```

### Exiftran

The tool `exiftran` doesn't delete EXIF data.

``` sh
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

``` sh
$ exiftran -i -9 <IMAGE>.jpg
```

Rotate counter clockwise:

``` sh
$ exiftran -i -2 <IMAGE>.jpg
```

## Exiftool

``` sh
# zypper in exiftool
```

Show currently set information of an image:

``` sh
$ exiftool <IMG>.jpg
```

Options:

-m
: ignore minor errors

-P
: preserve date

Get rid of 'Warning: Bad PreviewIFD directory':

``` sh
$ exiftool -m -P -overwrite_original -tagsfromfile @ -exifversion *.jpg
```

Reset modifydate to createdate:

``` sh
$ exiftool -P -overwrite_original -'filemodifydate<createdate' *.jpg
```

If not there already, set createdate:

``` sh
$ exiftool -P -overwrite_original -mwg:'createdate=YYYY:MM:DD hh:mm:ss' <IMG>.jpg
```

Adjust all dates, e.g. if camera time was incorrect:

``` sh
$ exiftool -AllDates+='HH:MM' -overwrite_original <IMG>.jpg
```

MWG Tags:

* copyright
* creator
* rating
* description
* keywords

Add some metadata:

``` sh
$ exiftool -m -P -overwrite_original \
  -mwg:creator='<NAME>' \
  -mwg:'copyright<Copyright $createdate, <NAME> (<ADDRESS>), all rights reserved' \
  -d %Y *.jpg
```

Add tags:

``` sh
$ exiftool -P -overwrite_original -mwg:keywords+='<TAG>' *.jpg
```

%%-c
: if multiple files add `-1`

%%+c
: if multiple files add `_1`

Sort pictures into subdirectories with filenames according to createdate:

``` sh
$ exiftool -r -P \
  -'Filename<DateTimeOriginal' \
  -d %Y-%m/%Y%m%d_%H%M%S%%-c.%%le *.jpg
```

## Thunar

Custom actions in `~/.config/Thunar/uca.xml`:

``` text
<?xml encoding="UTF-8" version="1.0"?>
<actions>
<action>
        <icon></icon>
        <name>Rotate clockwise</name>
        <unique-id></unique-id>
        <command>for FILE in %F; do exiftran -i -9 &quot;$FILE&quot;; done</command>
        <description></description>
        <patterns>*.jpg;*.JPG;*.jpeg;*.JPEG</patterns>
        <image-files/>
</action>
<action>
        <icon></icon>
        <name>Rotate counter clockwise</name>
        <unique-id></unique-id>
        <command>for FILE in %F; do exiftran -i -2 &quot;$FILE&quot;; done</command>
        <description></description>
        <patterns>*.jpg;*.JPG;*.jpeg;*.JPEG</patterns>
        <image-files/>
</action>
<action>
        <icon></icon>
        <name>Add metadata to images</name>
        <unique-id></unique-id>
        <command>addmetadata %F</command>
        <description></description>
        <patterns>*.jpg;*.JPG;*.jpeg;*.JPEG</patterns>
        <image-files/>
</action>
</actions>
```

## Transfer files with MTP

Installation:

``` sh
# zypper in simple-mtpfs
```

Show which devices are connected:

``` sh
$ simple-mtpfs -l
```

Mount:

``` sh
$ mkdir mnt
$ simple-mtpfs mnt
$ cd mnt
```

Unmount:

``` sh
$ cd
$ fusermount -u mnt
```
