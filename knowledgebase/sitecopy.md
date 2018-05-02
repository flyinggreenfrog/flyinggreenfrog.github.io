---
title: Sitecopy
last-changed: <time>2018-05-02</time>
knowledgebase: true
categories: [Linux, Network]
---
## Links

* [Arbeit mit Sitecopy](https://www.schlittermann.de/doc/sitecopy.html) <time>2018-05-02</time>

## Setup

Install sitecopy:

``` sh
# zypper in sitecopy
```

Create remote site information storage directory:

``` sh
$ mkdir -m 700 ~/.sitecopy
```

Configure sitecopy under `~/.sitecopyrc`:

``` text
# ~/.sitecopyrc
site <SITE>
server <HOSTNAME>
username <USER>
password <PASSWORD>
local <LOCALDIR>
remote <REMOTEDIR>
exclude <PATTERN>
state checksum
checkmoved renames
safe
```

Instead of specifying the password in `~/.sitecopyrc`, it can be specified in `~/.netrc` like for other ftp clients:

``` text
# ~/.netrc
machine <FTP-HOST> login <FTP-USER> password <FTP-PASSWORD>
```

Make config files only accessible for owner:

``` sh
$ chmod 600 ~/.sitecopyrc
$ chmod 600 ~/.netrc
```

## Usage

Download file list from server:

``` sh
$ sitecopy -f <SITE>
```

Compare local to remote:

``` sh
$ sitecopy <SITE>
```

Upload local changes to remote:

``` sh
$ sitecopy -u <SITE>
```