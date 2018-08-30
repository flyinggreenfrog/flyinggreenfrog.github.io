---
title: Screenshot
last-changed: <time>2018-08-30</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Gnome: Screenshots and screencasts](https://help.gnome.org/users/gnome-help/stable/screen-shot-record.html.en) <time>2018-08-30</time>

## Gnome Screenshot

### Options:

`-w`
: `--window`

`-a`
: `--area`

`-B`
: `--remove-border`

`-i`
: `--interactive`

`-f`
: `--file=<FILENAME>`

### Usage

`[Print]`
: Take screenshot of whole desktop

`[Alt] + [Print]`
: Take screenshot of window (`-w`)

`[Ctrl] + [Alt] + [Print]`
: Take screenshot of window without border (`-w -B`)

`[Shift] + [Print]`
: Take screenshot of area, that you have to select (`-a`)

## Shutter

``` sh
# zypper in shutter perl-Goo-Canvas
```

With shutter you can e.g. underline/mark parts in the screenshots or add
arrows or boxes. Generally you can add annotations or descriptions.

You can also open screenshots taken with other programs to edit them.

## Flameshot

``` sh
# zypper in flameshot
```

Disadvantage of flameshot is that you can't take a screenshot of only one
window.
