---
title: pre-commit
last-changed: <time>2020-09-21</time>
knowledgebase: true
categories:
  - Linux
  - Systemsmanagement
---
## Links

* [pre-commit](https://pre-commit.com) <time>2020-09-21</time>
* [pre-commit: Supported hooks](https://pre-commit.com/hooks.html) <time>2020-09-21</time>

## Setup

```console
$ pip3 install --user pre-commit==2.7.1
```

```console
$ git config --global init.templateDir ~/.git-template
$ pre-commit init-templatedir ~/.git-template
```

### Config

Example below is for usage with Ansible.

#### `.pre-commit-config.yaml`

```text
---
default_language_version:
  python: python3
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
      - id: check-added-large-files
  - repo: https://github.com/adrienverge/yamllint.git
    rev: v1.24.2
    hooks:
      - id: yamllint
        name: YAML Lint
        entry: yamllint --strict
        language: python
        types: [file, yaml]
  - repo: https://github.com/ansible/ansible-lint.git
    # In v4.2.0 auto-detection is working, but no longer with v4.3.X
    # https://github.com/ansible/ansible-lint/issues/1049
    rev: v4.2.0
    hooks:
      - id: ansible-lint
        name: Ansible Lint
        entry: ansible-lint
        #entry: ansible-lint . # for roles, until autodetection is working properly
        language: python
        # do not pass files to ansible-lint, see:
        # https://github.com/ansible/ansible-lint/issues/611
        pass_filenames: false
        always_run: true
```

## Usage

Create new repo:

```console
$ git init ~/example
$ cd ~/example
$ vim .pre-commit-config.yaml
$ ls -lh ~/.cache/pre-commit
$ pre-commit install --install-hooks
$ ls -lh ~/.cache/pre-commit
$ git add .pre-commit-config.yaml
$ git commit
```

Clone repo:

```console
$ git clone ~/example ~/example-clone
$ cd ~/example-clone
```

Run pre-commit manually:

```console
$ pre-commit run -a|--all-files
```

Skip checks:

```console
$ git commit --no-verify
$ SKIP=ansible-lint,yamllint git commit
```

```console
$ pre-commit uninstall ~/example
$ pre-commit clean
$ pre-commit gc
```
