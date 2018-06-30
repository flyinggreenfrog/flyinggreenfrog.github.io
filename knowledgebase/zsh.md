---
title: Zsh
last-changed: <time>2018-06-30</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Welcome to Zsh](http://www.zsh.org) <time>2018-05-31</time>
* [Z shell](http://zsh.sourceforge.net) <time>2018-05-31</time>
* [ZshWiki](http://zshwiki.org) <time>2018-05-31</time>

## Terms

Variables / Parameter
: Set: `var=value`, unset: `unset var`, access: `$var` or `${var}`

## Start

Startup files

* `/etc/zshenv`, `~/.zshenv`
* `/etc/zprofile`, `~/.zprofile`
* `/etc/zshrc`, `~/.zshrc`
* `/etc/zlogin`, `~/.zlogin`

## Options

Check which options are set or not set:

``` sh
$ setopt
$ unsetopt
```

## Aliases

Aliases are used to input commands more easily, e.g. as abbreviations.

### Normal aliases

``` sh
$ alias <ALIAS>=<COMMAND>
$ alias <ALIAS>='<COMMAND> <ARGUMENTS>'
$ alias <ALIAS>="<COMMAND> <ARGUMENTS>"
```

Quoting is not needed, if command is only one word. To quote you can use single
or double quotes, it depends, when variables should be replaced, either when
the alias is used or when the alias is defined.

``` sh
$ alias today='date -I'
$ today
2018-06-03
$ alias today
today='date -I'
$ which today
today: aliased to date -I
```

Useful aliases:

``` sh
$ alias ..='cd ..'
$ alias ...='cd ../..'
```

Alias can be used to correct indivdual typos, that you make often:

``` sh
$ alias dtae=date
```

Normal aliases are only expanded, if the alias is in the position of a
command, e.g. at the beginning of the line or after a pipe. Exception: a word
that is behind an alias, that ends with a space, will be checked too, if it is
an alias itself.

``` sh
$ print today
today
$ alias print='print '
$ print today
date -I
```

Aliases can be nested. If you want to avoid using aliases inside aliases you
have to prepend commands with the keyword *command*.

``` sh
$ alias ls='ls --color=auto'
$ ls
$ command ls
```

### Suffix aliases

If you type a string separated with dots, zsh looks if suffix aliases are
defined, if yes, it prepends the line with the suffix.

``` sh
$ alias -s txt=vim
$ test.txt
```

Normally you have to enter *vim test.txt*, with suffix alias defined the name
of the txt file is sufficient.

``` sh
$ alias -s de=$BROWSER com=$BROWSER org=$BROWSER
$ example.org
```

### Global aliases

Global aliases will be replaced everywhere in the line, if they are standing as
a word on its own.

``` sh
$ alias -g T=Mycommand
$ T
zsh: command not found: Mycommand
$ print T
Mycommand
$ print TT
TT
$ print \T
T
$ print 'T'
T
```

``` sh
$ alias -g NE='2> /dev/null'
$ alias -g NO='&> /dev/null'
$ alias -g G='| grep'
$ alias -g P='| $PAGER'
$ alias -g L='| less'
```

``` sh
$ sort <FILE> G <SEARCHSTRING> P
```

But global aliases have disadvantages too. If you want to insert something that
is already defined as global alias, and you may have forgotten, that you
defined this alias, this can lead to confusion. Therefore some people prefer to
expand global aliases automatically. This can be done with a widget function.

``` text
# Expand global aliases immediately
global-alias-space () {
  local ga="$LBUFFER[(w)-1]"
  [[ -n $ga ]] && LBUFFER[(w)-1]="${${galiases[$ga]}:-$ga}"
  zle self-insert
}
zle -N global-alias-space
bindkey ' ' global-alias-space
```

## Functions

Define function:

``` text
<FUNCTION>() {
  <COMMANDS>
  return [n]
}
```

There can be a space between function name and brackets in function definition.

Undefine function:

``` sh
$ unfunction <FUNCTION>
```

### Arguments

`$#`
: Number of arguments (same as `$ARGC`)

`$*`
: Array with all arguments

`$@`
: All arguments in words separated

`$1 - $9`
: Positional arguments, can be shifted with `shift`

`$0`
: Name of shell, when option `function_argzero` is set, then name of function

``` text
myfunc() {
  print Number of arguments: $#
  print All arguments: "$*"
  print -l Separated: "$@"
}
```

`print -l` uses newlines instead of spaces to separate arguments.

### Oneliner

Functions with only one line can be seen as extended aliases. With aliases
replacement is done before any arguments, with functions arguments can also be
inserted in the middle of a command.

``` text
wiki() { $BROWSER "http://de.wikipedia.org/wiki/$*" }
lsp() { ls --color=always -lh "$@" | less -r }
```

### Several names

If `function_argzero` option is set, `$0` is set to the name, that was used to
call the function, instead of the name of the shell.

``` text
Status Start Stop Restart Reload() {
  systemctl ${0:l} $1
}
```

## Commands

### Check

Use whence to check for each argument, how it would be interpreted, if used as
a command name.

which
: whence -c

type
: whence -v

``` sh
$ which <COMMAND>
$ type <COMMAND>
```

``` sh
$ alias
$ alias -s
$ alias -g
$ functions
```

### Edit

Edit aliases, suffix aliases, global aliases or functions on the fly:

``` sh
$ vared "aliases[<ALIAS>]"
$ vared "saliases[<SUFFIX-ALIAS>]"
$ vared "galiases[<GLOBAL-ALIAS>]"
$ vared "functions[<FUNCTION>]"
```

To save just press *Enter*, if you need to insert a literal *Enter*, press *ESC
+ Enter*.

## Expansion

``` sh
$ print {1..5}
$ print {1..5}[Tab]
$ print {1..5}[Ctrl]+[x][g]
```

### Brace Expansion

`{n..m}`
: Sequence of numbers, starting with `n`, ending with `m`.

`{a,b}`
: Separates each element of the collection to own word.

`{a-f}`
: If the shell option `brace_ccl` is set, consecutive letters can be
abbreviated with `-`.

Characters before and after the brackets, will be added before or after each
expanded word.

Sequence of numbers understands leading zeros, for each number leading zeros
will be prepended.

``` sh
$ print -l {1..5}
$ print -l {01..05}
```

``` sh
$ mv file.{txt,html}
```

Brace expansion vs. Globbing:

``` sh
$ ls Image{A-Z}*
$ ls Image[A-Z]*
```

### Filename expansion

``` sh
$ ls ~
$ ls ~/<DIR>
$ ls ~<USER>
```

Named directories:

``` sh
$ hash -d
$ hash -d log=/var/log
$ hash -d doc=/usr/share/doc
$ cd ~log
$ cd ~doc
```

Use `pushd` and `popd` to add directories to list of visited directories or
visit them again.  If shell option `autopushd` is set, every `cd` command adds
directory to that list of visited directories:

``` sh
$ dirs -v
```

`~+n`
: n-th entry from top

`~-n`
: n-th entry from bottom

To change meaning of `+` and `-` in this context, use shell option
`pushdminus`.

If shell option `cdable_vars` is set and you try to `cd` into a directory, that
doesn't exist, zsh tries filename expansion, even without leading `~`:

``` sh
$ cd /tmp
$ cd <USER>
$ pwd
/home/<USER>
```

If shell option `equals` is set, prepending `=` to a command (that is in `PATH`
variable), would replace it with the full path of the command:

``` sh
$ ls -lh=<CMD>
```

### Command substitution

``` text
$(<COMMAND>)

`<COMMAND>`

$(cat file.txt)
$(<file.txt)
```

`$( )` can be nested.

### Substitution of arithmetic expressions

``` sh
$ $((<TERM>))
$ $[<TERM>]
```

### Process substitution

`/proc/fd` mechanism (or named pipe on older systems)

``` sh
$ ... <(<COMMAND>)
$ ... >(<COMMAND>)
```

Example:

``` sh
$ convert <INPUT-FILE> -rotate 180 <OUTPUT-FILE>
$ convert <(wget -qO- http://example.org/image.jpg) \
  -rotate 180 >(ssh <USER>@<HOST> 'cat > rotated_image.jpg')
```

`<( )` and `>( )` don't support repositioning of the file descriptor offset.
But some programs may need that, then you can use a _temporary file variant_.

``` sh
$ ... =(<COMMAND>)
```

Example:

``` sh
$ <PDFVIEWER> =(bzcat text.pdf.bz2)
```

Compare two outputs with vimdiff
```
$ vimdiff =(ls -l <DIR1>) =(ls -l <DIR2)
```

## Globbing

Globbing or pattern matching produces from a pattern a list of
files/directories that matches on that pattern.

Regular expressions are not the same as globbing / pattern matching.

``` sh
$ print *.txt
$ print *.txt[Tab]
$ print *.txt[Ctrl]+[x][g]
```

``` sh
$ print "*.txt"
$ print '*.txt'
$ print \*.txt
```

To deactivate globbing use precommand modifier `noglob`:

``` sh
$ noglob print *.txt
```

Sometimes it's better to show globbing examples with `print` instead of `ls`,
because `ls` lists directory content instead of directory itself.

### Simple Globbing

``` sh
$ ls (.|)*
```

Glob operators

`*`
: Every string, including empty string, but no hidden files

`?`
: Every single character

`[...]`
: Every character within brackets

`[^....]`
: Every character not within brackets

`<m-n>`
: Numbers between m and n

`<m->`
: Numbers not less than m

`<-n>`
: Numbers not greate than n

`<->`
: Every Number

`( )`
: Brackets for grouping

`a|b`
: Pipe symbol for alternatives (only within brackets)


`<`, `>`, `|` are used for redirection and pipe, if not in complete expression above.

Examples:

``` sh
$ print *.(x|)htm(l|)
```

Hidden files:

``` sh
$ print .*
$ print (.|)*
$ print *(D) # qualifier, sets glob_dots
$ setopt glob_dots
$ print *
```

Alternative characters (POSIX character sets):

`[:alnum:]`
: Alphanumeric characters

`[:alpha:]`
: Character from alphabet

`[:ascii:]`
: 7-bit ASCII character

`[:blank:]`
: Space or tab

`[:digit:]`
: Numbers

`[:lower:]`
: small letters

`[:space:]`
: Space, tab or newline

`[:upper:]`
: Capital letters

To use these classes, put them in another set of square brackets. One set of
square brackets is already part of the POSIX character set.

``` sh
$ print *[[:space:]]*
$ mv Image_[_[:space:]]<->.[Jj][Pp][Gg] Images/
$ print *[^[:digit:]]*
```

Last expressions doesn't mean, find all without any number, but means find all
with at least one non-number.

Options:

``` sh
$ print doesnt*exist
zsh: no matches found: doesnt*exist
```

``` sh
$ setopt no_nomatch
$ print doesnt*exist
doesnt*exist
```

``` sh
$ setopt nullglob
$ print doesnt*exist

```

Instead setting nullglob globally you can use qualifier as well:

``` sh
$ print *.jp(e|)g(N)
```

### Extended Globbing

``` sh
$ setopt extended_glob
```

`^a`
: Exclude `a`

`a~b`
: `a`, but not `b`

`#`
: Repeat zero or more

`##`
: Repeat one or more

`**`
: Short for `(*/)#`

Exception examples:

``` sh
$ print (^.svn/)#*
$ print *~f.txt
$ print *~*.o
$ print *~*.txt~*<->*
```

Repetition examples:

``` sh
$ print [^[:digit:]]##
$ print *.????#
$ print *.?(??)#
```

Recursion examples:

``` sh
$ print (*/)#
$ print **
$ print (*/)#*.txt
$ print **/*.txt
```

When you encounter _argument list too long_, use zargs instead:

``` sh
$ ls **/*
$ zargs **/* -- ls
```

Globbing flags (used within `(# )` expression):

`i`
: Case insensitive

`l`
: Small letters in pattern match on capital letters, but capital letters in
pattern don't match on small letters

`I`
: Case sensitive again

`cm,n`
: Modification of `#` and `##` (use instead of them), repeat between m and n
times, m or n can be empty

`an`
: Up to `n` mistakes are allowed while constructing the name

``` sh
$ print *.[Jj][Pp][Gg]
$ print *.(#i)jpg
$ print *(#i)Image(#I)*.jpg
$ print *((#i)Image)*.jpg
```

Approximation errors can be:

* Mixed characters: `word` and `wort`
* Transposition of characters: `word` and `wrod`
* One character too less or too much: `word` and `words`

``` text
R() { less -- (#ia3)readme*; }
```

Several qualifier can be used after the expression in `( )` brackets.

Filetype qualifier:

`/`
: Directory

`F`
: Not empty directory

`.`
: Files (regular files)

`@`
: Symbolic link

`=`
: Socket

`p`
: Named pipe (FIFO)

`%`
: Device

`%b`
: Block device

`%c`
: Character device

``` sh
$ ls -l /bin/*(@)
$ ls -l /tmp/**/*(=)
```

Filemode/owner qualifier:

`f<OCTALMODE>`
: Octal mode rights

`rwx`
: User

`AIE`
: Group (reAd, wrIte, exEcute)

`RWX`
: Other

`s`
: setuid bit

`u<ID>`
: User id

`u[<NAME>]`
: User name

`g<ID>`
: Group id

`g[<NAME>]`
: Group name

Files not readable by owner:

``` sh
$ ls -l *(.^r)
```

Files from root, not readable from others:

``` sh
$ ls -l *(.u0^R)
```

``` sh
$ chmod a+r **/index.(#a1)(html|php)(.^R)
```

More qualifiers:

`^`
: Not

`-`
: Use following qualifiers on referenced file, if file is a link

`N`
: Set nullglob option, no error if pattern doesn't match

`D`
: Sets globdots options, includes hidden files, even when pattern doesn't start
with `.`

`L`
: Select according to size (`+` / `-` or `k` / `m`)

`a`, `m`, `c`
: Access / Modification / Change (inode) time (`+` / `-` or
`M` / `w` / `h` / `m` / `s`)

`oc`, `Oc`
: Sort / reverse sort according to order `c`

`[m]`
: Select m-th hit

`[m,n]`
: Selects from m-th to n-th hit

Sorting qualifiers (to use with `o` or `O`):

`n`
: Name

`L`
: Length

`a`, `m`, `c`
: Access / Modification / Change (inode)

`d`
: Files from directories first.

`N`
: None, no sorting.

``` sh
$ ls -l *(^/)
```

``` sh
$ ls -l *(.L0)
$ ls -l *(.Lm+1^Lm+10)
```

``` sh
$ print *(Dom[1])
.zsh_history
```

``` sh
$ print -l *(om[1])
$ print -l *(Om[-1])
```

Five newest files:

``` sh
$ ls -fl *(om[1,5])
```

Combine qualifier (and - or):

``` sh
$ print *(.R^Lk-50) # and
$ print *(.Lm-1,/^F) # or
```

## More examples

How many normal files are in the current dir with subdirs?

``` sh
$ files=(**/*(.)); echo ${#files}
```

Find all empty directories:

``` sh
$ print -l **/*(/^F)
```

Find all normal files bigger than 500MB

``` sh
$ ls **/*(.Lm+500)
```

Find all files not belonging to user tux

``` sh
$ ls **/*(^u[tux])
```

## zmv

``` sh
$ autoload -U zmv
```

Options:

`-n`
: Dry-run

`-v`
: Verbose

`-w`
: Pick out wildcard parts of the pattern and implicitly add parentheses

`-W`
: Same as `-w`, but also turn wildcard in replacement pattern into sequence

`-C`
: Copy instead of moving

Rename with prefix:

``` sh
$ zmv -W '*' '<PREFIX>*'
```

Capitalize filename and lowercase extension:

``` sh
$ zmv '(*).(*)' '${(C)1}.${(L)2}'
```

Add leading zeros and shift:

``` sh
$ ls
pic1.jpg pic2.jpg pic3.jpg

$ zmv 'pic(<1->).jpg' 'pic${(l:4::0:)$(($1 + 10))}.jpg'

$ ls
pic0011.jpg pic0012.jpg pic0013.jpg
```

## Programming

If:

``` text
if <CONDITION>; then
  <DO>
fi

if <CONDITION>; then
  <DO>
elif <CONDITION>; then
  <DO>
else
  <DO>
fi
```

For:

``` text
# simple (needs shell option short_loops)
for i ({1..9}) print $i

# standard
for i in {1..9}; do
  print $i
done
```
