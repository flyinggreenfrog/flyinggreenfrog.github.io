---
title: Python
last-changed: <time>2018-07-11</time>
knowledgebase: true
categories: [Programming]
---
## Links

* [Python documentation](https://docs.python.org) <time>2018-07-11</time>
* [The Python Wiki](https://wiki.python.org/moin) <time>2018-07-11</time>

## Terms

PEP
: Python Enhancement Proposal

PIP
: Python Installs Python

## PEPs

PEP 8
: Formatting of Python code

PEP 257
: Docstrings

## Setup

For installing additional python packages you can use `pip`. Before `pip` was
introduced, you used `easy_install`.

### Virtual environments

Create virtual environments with `venv` (Python 3):

``` sh
$ python3 -m venv <PATH_TO_NEW>/<ENVIRONMENT>
```

Create virtual environments with `virtualenv` (Python 2):

``` sh
# zypper in python-virtualenv
$ virtualenv <PATH_TO_NEW>/<ENVIRONMENT>
```

Activate virtual environment:

``` sh
$ source <PATH_TO_NEW>/<ENVIRONMENT>/bin/activate
(<ENVIRONMENT>) $ which python
```

Deactivate virtual environment:

``` sh
(<ENVIRONMENT>) $ deactivate
$
```

## Python 2 vs. Python 3

Datatypes:

* Python 2: `int` limited, `long` unlimited
* Python 3: `int` unlimited, no `long` anymore

## Syntax

Variables:

``` text
^[a-zA-Z][a-zA-Z0-9_]*$
```

Keywords:

``` sh
>>> import keyword
>>> keyword.kwlist
```

### if

``` text
if <CONDITION>:
    <INSTRUCTION>
```

## Logging

``` sh
# zypper in python-systemd
```
