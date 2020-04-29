---
title: Git
last-changed: <time>2020-04-29</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [Book Pro Git](http://git-scm.com/book) <time>2018-05-07</time>
* [Git Best Practices](http://sethrobertson.github.io/GitBestPractices) <time>2018-05-07</time>
* [A Git Horror Story: Repository Integrity With Signed Commits](https://mikegerwitz.com/papers/git-horror-story) <time>2018-05-07</time>

## Terms

SHA
: Secure Hash Algorithm

Reflog
: Reference Log

Refspec
: Reference Specification

## Usage

### Working with forks

Configure a remote for a fork:

``` sh
$ git remote -v
$ git remote add <UPSTREAM> <URL>/<OWNER>/<ORIGINAL_REPOSITORY>.git
$ git remote -v
```

Sync a fork:

``` sh
$ git fetch <UPSTREAM>
$ git checkout master|<BRANCH>
$ git merge --ff-only upstream/master|upstream/<BRANCH>
$ # If nothing is pushed yet, you can rebase instead of merging
$ git rebase upstream/master|upstream/<BRANCH>
```
