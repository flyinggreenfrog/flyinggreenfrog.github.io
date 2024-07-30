---
title: Git
last-changed: <time>2024-07-02</time>
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

### Checkout remote branch

```console
$ git checkout -t origin/<BRANCH>
```

### Working with forks

Configure a remote for a fork:

```console
$ git remote -v
$ git remote add <UPSTREAM> <URL>/<OWNER>/<ORIGINAL_REPOSITORY>.git
$ git remote -v
```

Sync a fork:

```console
$ git fetch <UPSTREAM>
$ git checkout master|<BRANCH>
$ git merge --ff-only upstream/master|upstream/<BRANCH>
$ # If nothing is pushed yet, you can rebase instead of merging
$ git rebase upstream/master|upstream/<BRANCH>
```

### Split a commit into smaller ones

```console
$ git log --oneline
$ git rebase -i <COMMIT-HASH-BEFORE-COMMIT-TO-BE-CHANGED>
... pick -> edit ...
$ git reset HEAD~1
$ git commit
$ [git commit]
$ git rebase --continue
```

If something goes wrong and you want to start again:

```console
$ git rebase --abort
```

### Delete a specific commit

```console
$ git log --oneline
$ git rebase -i <COMMIT-HASH-BEFORE-COMMIT-TO-BE-DELETED>
... pick -> drop ...
$
```
