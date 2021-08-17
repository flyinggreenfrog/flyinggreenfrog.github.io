---
title: todotxt
last-changed: <time>2021-03-21</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [todo.txt](http://todotxt.org) <time>2021-01-29</time>
* [todo.txt-cli](https://github.com/todotxt/todo.txt-cli) <time>2021-01-29</time>
* [todo.txt format](https://github.com/todotxt/todo.txt) <time>2018-08-03</time>
* [A Pragmatic Guide to Getting Things Done](https://hamberg.no/gtd) <time>2018-08-03</time>
* [Plaintext Productivity Tasks](http://plaintext-productivity.net/1-00-tasks-introduction.html) <time>2018-08-03</time>
* [How to GTD with Simpletask](https://gist.github.com/alehandrof/9941620) <time>2018-08-03</time>
* [My New Paperless To Do List Management System](http://www.jamierubin.net/2012/08/23/my-new-paperless-to-do-list-management-system) <time>2018-08-03</time>
* [todo.txt for great profit](http://elsherbini.org/todo-txt-for-great-profit) <time>2018-08-03</time>
* [Vim plugin for Todo.txt](https://github.com/freitass/todo.txt-vim) <time>2018-08-03</time>
* [Todotxt-machine: terminal based editor](https://github.com/AnthonyDiGirolamo/todotxt-machine) <time>2018-08-03</time>
* [Gnome shell interface for todo.txt](https://extensions.gnome.org/extension/570/todotxt) <time>2018-08-03</time>
* [Simpletask Android](https://github.com/mpcjanssen/simpletask-android/blob/master/app/src/main/assets/index.en.md) <time>2018-08-03</time>
* [How to manage todo.txt with ownCloud](https://blog.portknox.net/content/all/?q=todo-txt) <time>2018-08-03</time>

## Getting things done with todotxt

Getting Things Done:

1. Capture
2. Clarify
3. Organize
4. Reflect
5. Engage

Resolve open loops, manage actions and get things done.

Outcome
: Define, what done means

Action
: Define, what doing looks like

Terms:

* Context
* Project
* Priority

Contexts (lists):

* No context means it's in "in" (`@in`)
* Next actions (sorted by context)
  - `@home`
  - `@computer`
  - `@mail`
  - `@call`
  - `@online`
  - `@outside`
  - `(@everywhere)`
* `@waitingfor`
* `@someday` / `@maybe`
* `@project`: for more info about a project tag
* Agenda contexts
  - `@agenda_<PERSON>`
  - `@agenda_<MEETING>`

Projects (tags):

* `+cleanGarage`
* `+finances`
* `+knowledge`
* `+personal`
* `+family`
* `+wife`
* `+work`

Format:

```text
[x] [(<PRIO>)] [<COMPLETION-DATE>] <CREATION-DATE> @<CONTEXT> do something for +<PROJECT>
```

Tools:

* Calendar
* Trigger list
* Read/review folder
* Tickler file
* Notes to tasks / project support

Weekly Review:

* Make sure every project has at least one next action
* Move things you won't do soon to someday/maybe
* Move things from someday/maybe to next actions
* Define new projects

Priorities:

* A: task working on now
* B: tasks to plan to do today
* C: tasks to plan to do this week

## taskwarrior / timewarrior

## todo.txt-cli

* `todo.txt`
* `done.txt`
* `report.txt`

Add todo:

```console
$ todo.sh add|a "[@<CONTEXT>] <MYTODO> [+<PROJECT>]"
```

Add todo to another file:

```console
$ todo.sh addto <DESTINATION>.txt "..."
```

Add prioritized todo:

```console
$ todo.sh aa "..."
$ todo.sh ab "..."
$ todo.sh ac "..."
```

Mark todo as done:

```console
$ todo.sh do <TODONUMBER>
```

Delete todo:

```console
$ todo.sh del|rm <TODONUMBER> [<TERM2REMOVE>]
```

Move todo to another list:

```console
$ todo.sh mv <TODONUMBER> <DESTINATION> [<SOURCE>]
```

Show todos:

```console
$ todo.sh list|ls [<SEARCH>]
```

Show todos without <SEARCH>:

```console
$ todo.sh list|ls -<SEARCH>
```

Show all todos including done todos:

```console
$ todo.sh listall|lsa [<SEARCH>]
```

Show todos from another file:

```console
$ todo.sh listfile|lf <DESTINATION> [<SEARCH>]
```

Show prioritized todos:

```console
$ todo.sh listpri|lsp [<PRIORITY>]
```

Show all used contexts:

```console
$ todo.sh listcon|lsc
```

Show all used projects:

```console
$ todo.sh listproj|lsprj
```

Prioritize:

```console
$ todo.sh pri|p <TODONUMBER> <PRIORITY>
```

Deprioritize:

```console
$ todo.sh pri|dp <TODONUMBER>
```

Archive (move done todos to `done.txt` and remove blank lines):

```console
$ todo.sh archive
```

Add number of open and done todos to `report.txt`:

```console
$ todo.sh report
```

## Plugins

### edit

```console
$ todo.sh edit
$ todo.sh edit <DESTINATION>
```

### note

```console
$ todo.sh note
```

### due

Add `due:YYYY-MM-DD` to entries, while adding or editing.

List due todos:

```console
$ todo.sh due
$ todo.sh due 7
```

### revive

```console
$ todo.sh revive
$ todo.sh revive <TODONUMBER>
```

### graph

```console
$ todo graph
```

## vim

There is a vim plugin for todo.txt

Sorting tasks:

`<localleader>s`
: Sort the file

`<localleader>s+`
: Sort the file on `+projects`

`<localleader>s@`
: Sort the file on `@contexts`

`<localleader>sd`
: Sort the file on dates

`<localleader>sdd`
: Sort the file on due dates

Edit priority:

`<localleader>j`
: Decrease priority of the current line

`<localleader>k`
: Increase priority of the current line

`<localleader>a`
: Add priority (A) to the current line

`<localleader>b`
: Add priority (B) to the current line

`<localleader>c`
: Add priority (C) to the current line

Date:

`<localleader>d`
: Set creation date of current line to current date

`date<TAB>`
: (Insert Mode) Insert the current date

Mark as done:

`<localleader>x`
: Mark current line as done

`<localleader>X`
: Mark all lines as done

`<localleader>D`
: Archive
