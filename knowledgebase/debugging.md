---
title: Debugging
last-changed: <time>2018-05-02</time>
knowledgebase: true
categories: [Programming]
---
## Links

* [Debugging with GDB](http://sourceware.org/gdb/current/onlinedocs/gdb) <time>2018-05-02</time>
* [GDB Tutorial](http://www.dirac.org/linux/gdb) <time>2018-05-02</time>
* [gdb Debugger Tutorial](http://www.unknownroad.com/rtfm/gdbtut/gdbtoc.html) <time>2018-05-02</time>
* [Guide to Faster, Less Frustrating Debugging](http://heather.cs.ucdavis.edu/~matloff/UnixAndC/CLanguage/Debug.html) <time>2018-05-02</time>

## Usage

``` sh
$ gdb -q <PROG>
(gdb) r|run
(gdb) l|list
(gdb) b|break
(gdb) c|continue
(gdb) disp|display
(gdb) p|print
(gdb) n|next
(gdb) s|step
(gdb) bt|backtrace
(gdb) set var VARIABLE = VALUE
```
