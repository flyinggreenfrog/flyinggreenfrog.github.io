---
title: Sitecopy
last-changed: <time>2020-10-07</time>
knowledgebase: true
categories: [Linux, Network]
---
## Links

* [Arbeit mit Sitecopy](https://www.schlittermann.de/doc/sitecopy.html) <time>2020-10-07</time>

## Setup

Install sitecopy:

```console
# zypper in sitecopy
```

Create remote site information storage directory:

```console
$ mkdir -m 700 ~/.sitecopy
```

Configure sitecopy under `~/.sitecopyrc`:

```text
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

```text
# ~/.netrc
machine <FTP-HOST> login <FTP-USER> password <FTP-PASSWORD>
```

Make config files only accessible for owner:

```console
$ chmod 600 ~/.sitecopyrc
$ chmod 600 ~/.netrc
```

## Usage

Download file list from server:

```console
$ sitecopy -f <SITE>
```

Compare local to remote:

```console
$ sitecopy <SITE>
```

Upload local changes to remote:

```console
$ sitecopy -u <SITE>
```
