---
title: Latex
last-changed: <time>2018-09-23</time>
knowledgebase: true
categories: [Linux]
---
## latexmk

### Config

#### `~/.latexmkrc`

``` text
$pdf_mode = 1;
$pdf_previewer = 'start zathura';
```

### Usage

#### Run a previewer and continually update it

``` sh
$ latexmk -pvc <FILE>.tex
```

#### Clean all except pdf

``` sh
$ latexmk -c
```

#### Clean all

``` sh
$ latexmk -C
```
