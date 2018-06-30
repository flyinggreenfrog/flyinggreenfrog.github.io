---
title: Git Annex
last-changed: <time>2018-06-30</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [git-annex](http://git-annex.branchable.com) <time>2018-06-30</time>
* [Dateien verteilen und verwalten mit Git Annex](http://www.linux-magazin.de/Ausgaben/2013/11/Git-Annex) <time>2018-06-30</time>

## Terms

Annex
: Extension

## Setup

``` sh
# zypper in git git-annex
```

For now use git-annex repo version 5, since version 6 still seems a little bit
buggy (e.g. sqlite errors).

Create main repo:

``` sh
$ mkdir ~/annex
$ cd ~/annex
$ git init
$ git-annex init [<DESCRIPTION>] --version=5
```

Create another repo:

``` sh
$ cd /media/usb
$ git clone ~/annex
$ cd annex
$ git-annex describe here "usb drive"
$ git remote rename origin annex
$ cd ~/annex
$ git remote add usbdrive /media/usb/annex
```

Change description:

``` sh
$ git-annex describe here <DESCRIPTION>
```

Reinit / reusing annex.uuid / restore after e.g. whole repo was deleted:

``` sh
$ mv ~/annex ~/annex_old
$ mkdir ~/annex
$ cd ~/annex
$ git init
$ git config annex.uuid <ANNEX-UUID>
$ git-annex init [<DESCRIPTION>] --version=5
$ git remote add usbdrive /media/usb/annex
$ git-annex sync
```

## Usage

Add files:

``` sh
$ git-annex add <FILES>
$ git commit -m 'Annexed files.'
$ git-annex sync
```

Revert accidental add (with version 5 you first have to commit/sync):

``` sh
$ git-annex add <FILES>
$ git commit -m 'Annexed files.'
$ git-annex unannex <FILES>
$ git commit -m 'Unannexd files.'
```

Unlock file, edit it and annex/commit again:

``` sh
$ git-annex unlock <FILE>
$ vim <FILE>
$ git-annex add <FILE>
$ git commit -m 'Edited file.'
```

Discard changes to an unlocked file:

``` sh
$ git-annex unlock <FILE>
$ git-annex lock [--force] <FILE>
```

Bring things already in git now to git-annex:

``` sh
$ git rm --cached <FILE>
$ git commit -m 'Removed from git.'
$ git-annex add <FILE>
$ git commit -m 'Annexed file.'
```

Remove things from git-annex, while keeping working copy:

``` sh
$ git unannex <FILE>
$ git commit -m 'Removed from git-annex.'
```

Find and fix problems:

``` sh
$ git-annex fsck --fast --quiet|-q
```

Show which remotes contain what:

``` sh
$ git-annex whereis [<PATH>]
```

More compact view:

``` sh
$ git-annex list [<PATH>]
```

Unused files (not referenced in branches or tags):

``` sh
$ git-annex unused
$ git-annex move --unused --to <REPO>
```

### Groups of repositories

List groups of a repo:

``` sh
$ git-annex group <REPO>
```

Get or set preferred content to use group defaults:

``` sh
$ git-annex wanted <REPO> [groupwanted]
```

Add repo to group:

``` sh
$ git-annex group <REPO> <GROUP>
```

Get or set expression to group:

``` sh
$ git-annex groupwanted <GROUP> [<EXPRESSION>]
```

``` sh
$ git-annex move <FILES> --to <DRIVE>
$ git-annex copy <FILES> --to <DRIVE>
$ git-annex get <FILES>
$ git-annex drop <FILES>
```

## Config

Edit git-annex config:

``` sh
$ git-annex vicfg
```

How many copies of a file should be stored?

``` sh
$ git annex numcopies <N>
```

Config file `.gitattributes` in individual directories:

``` text
* annex.numcopies=<N>
```
