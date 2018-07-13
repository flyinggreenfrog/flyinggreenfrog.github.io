---
title: Python
last-changed: <time>2018-07-13</time>
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

* [Python: Python Language Reference](https://docs.python.org/3/reference/index.html) <time>2018-07-12</time>

Variables:

``` text
^[a-zA-Z][a-zA-Z0-9_]*$
```

Keywords:

``` sh
>>> import keyword
>>> keyword.kwlist
```

### Control structures

``` text
if <CONDITION>:
    <INSTRUCTION>
```

## Regular Expressions

* [Python: re - regular expression operations](https://docs.python.org/3/library/re.html) <time>2018-07-12</time>
* [Python: Regular Expression HOWTO](https://docs.python.org/3/howto/regex.html) <time>2018-07-12</time>

RE, regexp
: regular expression

Sometimes string functions are better (faster) than RE, but RE are more
powerful, so decide on your own, what you really need.

Matching vs. searching in RE, searching is more complex.

RE use backslash character `\` as special sequence or for escaping. In
string literals Python is doing the same, so it's best that you use raw strings
for RE in Python.

``` text
'\n' one-character string containing newline
r'\n' two-character string containing '\' und 'n'

\text       text string to be matched
\\text      escaped backslash for RE
'\\\\text'  escaped backslash for string literal
r'\\text'   in raw string no extra escaping needed for RE
```

### Matching Characters

Normal characters match case sensitive:

``` text
r'test' matches test
```

Metacharacter:

``` text
. ^ $ * + ? { } [ - ] \ | ( )

[] character classes, some metacharcter loose their meaning inside character
  classes

  - range in character class, if there not first/last character or escaped

  ^ not in character class, if there first character

\ escape character or starting special sequence

. any character except newline (except if flag re.DOTALL is set)

| alternatives

^  begin of string (after each newline, if flag re.MULTILINE is set)

$  end of string or before newline at end of string (before each newline,
   if flag re.MULTILINE is set)

\A begin of string

\Z end of string

\b begin of word

\B end of word

( ) groups

  \<NR> backreference to group (0 <= <NR> <= 99)

  \g<NR> backreference in replacement string

(? ) extensions (some are groups, others not)

  (?: ) non-capturing group, can be useful in modifying a pattern

  (?# ) comment, will be ignored

  (?= ) positive lookahead assertion

  (?! ) negative lookahead assertion

  (?<= ) positive lookbehind assertion

  (?<! ) negative lookbehind assertion

  (?(id/name)yes|no)

    re.sub(r'(<)?test(?(1)>|(?<!<test)(?!>|\w))', 'TEST', '<test> <test test! test> test')

(?aiLmsux) see flags below

(?P ) python extensions of extensions

  (?P<word> ) named group 'word'

  (?P=word) backreference for named group 'word'

  \g<word> backreference in replacement string for named group 'word'
```

Special sequences:

``` text
\d digits
\D non-digits
\s whitespace
\S non-whitespace
\w alphanumeric
\W non-alphanumeric
```

Flags (in extensions or as parameter):

``` text
(?a) re.A re.ASCII ASCII-only instead of Unicode matching
(?i) re.I re.IGNORECASE case-insensitive matching
(?L) re.L re.LOCALE some special sequences depend on current locale (use is discouraged)
(?m) re.M re.MULTILINE change behavior of ^ and $
(?s) re.S re.DOTALL make . also match newline
(?u) re.U re.UNICODE only for backword compatibilty, redundant in Python 3
(?x) re.X re.VERBOSE allow whitespace and # as comment
```

As parameters flags can be combined with binary operator `|`.

'Write-only' RE vs. `re.VERBOSE`:

``` text
p = re.compile(r'''
  \s*     # Beginning whitespace
  (\S*)   # Grouping non-whitespace
  \s*$    # Trailing whitespace
''', re.VERBOSE)
m = p.match(' test ')
print(m.group())
print(m.groups())
```

Write-only: after writing RE no one else understands them again ;-)

### Repeating Characters

Repeating is greedy by default (in brackets non-greedy version):

``` text
* (*?)   zero or more times
+ (+?)   one or more times
? (??)   zero or one times
{m,n} ({m,n}?)
* = {0,}
+ = {1,}
? = {0,1}
```

RE are greedy by default:

``` text
Regexp: a[bcd]*b
String: abcbd
```

### Grouping

Groups are created with `(` and `)` inside a pattern.

### Using regular expressions

Interactive tool for checking RE (needs tkinter):

``` sh
# zypper in python3-tk
$ wget https://raw.githubusercontent.com/python/cpython/3.7/Tools/demo/redemo.py -O ~/bin/redemo
$ chmod 500 ~/bin/redemo
$ redemo
```

Using RE with compiling:

``` sh
>>> import re
>>> p = re.compile(r'ab*')
>>> p
re.compile('ab*')
>>> m = p.findall('abbcd')
['abb']
```

Using RE without compiling:

``` sh
>>> import re
>>> re.findall(r'ab*', 'abbcd')
['abb']
```

Pattern object methods / module-level functions:

``` text
p.match() re.match()
  check, if RE matches at beginning of string, returns None or match object
p.fullmatch() re.fullmatch()
  check, if RE matches on complete string
p.search() re.search()
  check, if RE matches within string, returns None or match object
p.findall() re.findall()
  find all substrings, where RE matches, returns list
p.finditer() re.finditer()
  find all substrings, where RE matches, returns iterator
p.split() re.split()
  split string into list according to separator RE
p.sub() re.sub()
  find and replace RE matches with another string
p.subn() re.subn()
  same as sub(), but returns additionally umber of replacements
```

Match objects methods / attributes:

``` text
m.group()       return string matched
m.group(1)
m.group('word')
m.groups()      return tuple with all groups matched
m.groupdict()   return named groups
m.start()       return start position of match
m.end()         return end position of match
m.span()        return (start, end) tuple of match
m.re            RE used to produce this match object
m.string        string passed to match the RC
```

## Logging

``` sh
# zypper in python-systemd
```

## Introduction into object oriented programming

### Objects, classes, methods, attributes

Object has attributes and methods.

Access over `.` on reference of object.

Class is description of an object like a recipe.
The real object itself then is an instance of a class.

Class definition:

``` text
# A.py
class A:
    pass
```

``` sh
>>> from A import *
>>> a = A()
```

Methods:

``` text
class A:
    def method1(self):
        pass
    def method2(self, arg):
        pass
```

``` sh
>>> a.method1()
>>> a.method2('argument')
```

Parameter `self` is the instance on the left side of `.`.

Constructor:

``` text
class A:
    def __init__(self):
        print('constructor of A')
```

``` sh
>>> A()
```

No destructor, that will be called definitely. Finalizer `__del__()` is
similar, but is not guaranteed to run.

Attributes should be defined inside constructor.

``` text
class A:
    def __init__(self, x):
        self.x = x
```

``` sh
>>> a = A(42)
>>> a.x
42
```

### Inheritance and polymorphism

A derived class inherits from a base class.

``` text
class A:
    pass

class B(A):
    pass
```

Polymorphism of class methods:

``` text
class A:
    def __init__(self):
        print('contructor of A')

class B(A):
    del __init__(self):
        super().__init__()
        #A.__init__()
        print('constructor of B')
```

Multiple inheritance:

``` text
class A:
    pass
class B:
    pass

class C(A, B):
    pass
```

### Getter, setter, property attributes

``` text
class A:
    def __init__(self):
        self._x = 42
    def getx(self):
        return self._x
    def setX(self, x):
        self._x = x
```

Naming the attribute `_x` is by convention with underscore for attributes, that
are seen as implementation detail, but they are not specially protected by
Python, even the use of getter/setter methods doesn't change that.

Property Attributes:

``` text
class A:
    def __init__(self, x=42):
        self._x = 42
    def getx(self):
        return self._x
    def setx(self, x):
        self._x = x
    x = property(getx, setx)
```

### Static methods, class methods and class attributes

``` text
def m():
    print('static method')

class A:
    m = staticmethod(m)
```

``` sh
>>> A.m()
```

Static methods can be used to provide alternative constructors:

``` text
def a():
    _a = A()
    _a.x = int(_a.x / 2)
    return _a

class A:
    ...
    a = staticmethod(a)
```

``` sh
>>> a = A.a()
```

Class methods can be called with instance:

``` text
class A:
    def c(cls):
        print(cls)
    c = classmethod(c)
```

``` sh
>>> A.c()
>>> a = A()
>>> a.c()
```

Class methods are often used with metaclasses.

Like class methods you can use class attributes:

``` text
class A:
    d = 42
```

``` sh
>>> A.d
>>> a = A()
>>> a.d
```

### Builtin functions for objects

``` text
a.x   getattr(a, 'x')
a.x = v   setattr(a, 'x', v)
hasattr(a, 'x')
del x.y   delattr(a, 'x')
isinstance(a, A)
issubclass(B, A)
```

### Exception handling

* [Python: Built-in Exceptions](https://docs.python.org/3/library/exceptions.html) <time>2018-07-12</time>
* [Python Tutorial: Errors and Exceptions](https://docs.python.org/3/tutorial/errors.html) <time>2018-07-12</time>

Functions have following alternatives what they can do with an exception from a
subfunction:

* Catch exception, handle it, resume normally
* Catch exception, handle it, throw exception again, calling function reacts
* Don't catch exception, calling function reacts

Builtin exceptions:

``` text
NameError
SyntaxError
TypeError
```

Class `BaseException` is base class for all exceptions. For own exceptions use
`Exception` as class to inherit from.

``` sh
>>> e = Exception('myerror')
>>> e.args
```

``` sh
>>> raise SyntaxError('myerror')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
SyntaxError: myerror
```

``` text
try:
    raise Exception('myerror')
except Exception as e:
    print(e.args[0])
```

Several exceptions can be noted as tuple after `except` keyword or with
different `except` statements.

``` text
try:
    ...
except <EXCEPTION1> as <NAME1>:
    ...
except <EXCEPTION2> as <NAME2>:
    ...
else:
    ...
finally:
    ...
```

Own Exceptions:

``` text
class Error(Exception):
    def __init__(self, msg):
        self.msg = msg
    def __str__(self):
        return 'myerror'
```

If you don't handle an exception you will see the stack trace
(callstack / traceback / backtrace).

With `raise` in an `except` statement you can handle it, but then raise the
same exception again, while the stack trace remains the same.

Exception chaining: raise another exception in an `except` statement.

With `raise <EXCEPTION2> from <EXCEPTION1>` you can give an exception context.

With `raise <EXCEPTION> from None` you prevent the Exception chaining.

## Advanced object oriented programming

### Magic methods and magic attributes

* [Python: Dunder Alias](https://wiki.python.org/moin/DunderAlias) <time>2018-07-13</time>
* [Python: Special methods names](https://docs.python.org/3/reference/datamodel.html#special-method-names) <time>2018-07-13</time>

The name of magic methods and magic attributes start and end with two
underscores `__`.

They are normally not called explicitly, but implicitly. That's why they are
called _magic_.

`__init__` can be pronounced as _dunder init_ as abbreviation for _double
underscore init_.

E.g. for _operator overloading_ you can implement the `__add__` method for your
own classes.

General magic methods:

``` text
__init__    Constructor
__del__     Finalizer (improperly destructor)
__repr__
__str__
__bytes__
__format__
__bool__
__complex__
__int__
__float__
__call__    To define callable objects
```

Attribute access:

``` text
__dict__          dict attribute for members of an instance
__getattr__
__getattribute__
__setattr__
__delattr__
__slots__         attribute for members to avoid dynamic __dict__
```

Comparison operators:

``` text
<   __lt__
<=  __le__
==  __eq__
!=  __ne__
>   __gt__
>=  __ge__
```

Binary operators:

``` text
+   __add__ __radd__
-   __sub__ __rsub__
*   __mul__ __rmul__
@   __matmul__ __rmatmul__ (matrix multiplication)
/   __truediv__ __rtruediv__
//  __floordiv__ __rfloordiv__
%   __mod__ __rmod__
    __divmod__ __rdivmod__ (divmod() returns tuple (//, %))
**  __pow__ __rpow__ (with 3rd arg supports pow()
<<  __lshift__ __rlshift__
>>  __rshift__ __rrshift__
&   __and__ __rand__
^   __xor__ __rxor__
$   __or__ __ror__
```

Inplace operators:

``` text
+=  __iadd__
-=  __isub__
*=  __imul__
@=  __imatmul__ (matrix multiplication)
=/  __itruediv__
=// __ifloordiv__
=%  __imod__
**= __ipow__
=<< __ilshift__
=>> __irshift__
&=  __iand__
^=  __ixor__
$=  __ior__
```

Unary operators:

``` text
-   __neg__
+   __pos__
abs __abs__
~   __invert__
```

Implement built-in functions:

``` text
__dir__
__hash__
__round__
__trunc__
__floor__
__ceil__
```

## Advanced programming

### Container, sequences and mappings

* [Python: collections - Container datatypes](https://docs.python.org/3/library/collections.html#module-collections) <time>2018-07-13</time>

If Python's standard containers (like `list`, `dict`, `set` or `tuple`) is not
enough have a look at the module `collections`.

Alternatively you can attach container functionality to own classes.

To emulate container types implement:

``` text
__len__
__length_hint__
__getitem__
__missing__
__setitem__
__delitem__
__iter__
__reversed__
__contains__
```

Sequences and mappings are both container types. Sequences have in integer as
index, mappings have more general indices. E.g. lists are sequences and dicts
are mappings.

Sequences should implement magic methods for addition (chaining) and
multiplication (repetition).

Mutable sequences should implement following methods:

``` text
append()
count()
index()
extend()
insert()
pop()
remove()
reverse()
sort()
```

Mappings should implement following methods:

``` text
keys()
values()
items()
get()
clear()
setdefault()
pop()
popitem()
copy()
update()
```
