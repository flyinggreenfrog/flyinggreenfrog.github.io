---
title: user
last-changed: <time>2020-11-25</time>
knowledgebase: true
categories: [Linux]
---
## Usage

Change username (groupname only when using private user groups):

```console
# usermod -l <USER_NEW> -m -d <HOME_NEW> <USER_OLD>
# groupmod -n <USER_NEW> <USER_OLD>
```

Verify, that sudo rules for that user are still valid:

```bash
# visudo
```
