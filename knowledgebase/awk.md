---
title: Awk
last-changed: <time>2018-11-05</time>
knowledgebase: true
categories: [Linux, Programming]
---
## Links

* [Awk](http://www.grymoire.com/Unix/Awk.html) <time>2018-11-05</time>

## Terms

AWK
: Aho, Weinberger, Kernighan

FNR
: File number records, reset to 0 with each new file

FS
: Field separator, default is space `" "`, whitespace at begin/end is ignored

NF
: Number of fields

NR
: Number records

OFMT
: Output format for numbers, default is `"%.6g"`

OFS
: Output field separator, default is space `" "`

ORS
: Output record separator, default is new line `"\n"`

RS
: Record separator, default is new line `"\n"`

RT
: Record text

## Description

Awk knows 2 datatypes, numbers and strings (and hashes/arrays of them).

Comments are allowed at the end of every line:

``` text
# This is a comment
```

Strings:

``` text
""            # empty string
"string1"     # without newline
"string2\n"   # with newline
```

Logic: `0` and `""` are false, everything else (e.g. `"1"`) is true.

Programs contain:

* Rules: `PATTERN { ACTION }` pairs
* Function definitions

Pattern:

* `BEGIN`
* `END`
* logic expression
* regular expression
* range pattern

``` text
BEGIN
Rules
END
Functions
```

Fields:

``` text
$0
$1 - $NF
NF
```

Standard actions:

``` text
{ print }
{ print $0 }
```

Define variable on commandline:

``` sh
$ awk -v VAR="$var" '...'
```

Include awk scripts in others:

``` text
@include "FILENAME.awk"
```

Metacharacters:

``` text
^
$
.
[ ]
[^ ]
[ - ]
[[:space:]] [[: :]]
|
( )
*
+
?
{n}
{n,}
{n,m}
```

Case Sensitive:

``` text
tolower($0) ~ /regex/   # gawk only
```

Functions:

``` text
asort(source [, dest [, how]])    # gawk only

asorti(source [, dest [, how]])   # sort indices, gawk only

index(source, target)

length([string=$0])

match(string, regexp [, array])

sub(regexp, replacement, [, target=$0])
 return #substitutions 0 or 1

gsub(regexp, replacement [, target=$0]) # global sub
 return #substitutions

gensub(regexp, replacement, how [, target=$0]) # general substitution function, gawk only

substr(string, start [, length])
```

Matching patterns:

``` text
var = gensub(/(.+) (.+)/, "\\2 \\1", "g"); print var
```

Comparisons / Matching Operators:

``` text
~
!~
```

Printing output:

``` text
print
printf
```

## Usage

### `test.awk`

``` text
#!/usr/bin/gawk -f
```

``` sh
$ chmod +x test.awk
$ ./test.awk
```
