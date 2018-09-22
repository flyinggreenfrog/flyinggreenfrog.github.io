---
title: Subversion
last-changed: <time>2018-09-22</time>
knowledgebase: true
categories: [Linux, Programming]
---
## Links

* [Version Control with Subversion](http://svnbook.red-bean.com) <time>2018-05-02</time>

## Usage

View diff

``` sh
$ svn diff -r <REV1>[:<REV2>]
```

``` sh
$ echo "CMakeModules http://svn.example.org/modules" > foo.txt
$ svn ps svn:externals . -F foo.txt
$ rm foo.txt
```
