---
title: GPG
last-changed: <time>2018-05-06</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [The GNU Privacy Guard](https://gnupg.org) <time>2018-05-06</time>

## Terms

DSA
: Digital Signature Algorithm

GPG
: GNU Privacy Guard

HSM
: Hardware Security Module

PKI
: Public Key Infrastructure

RSA
: Rivest-Shamir-Adleman Cryptosystem

WOT
: Web of Trust

## Daily Usage

Encrypt a file:

``` sh
$ gpg --armor --encrypt --recipient <KEYID>|<MAIL> <FILE>
$ shred -u <FILE>
$ file <FILE>.asc
<FILE>.asc: PGP message Public-Key Encrypted Session Key (old)
```

Decrypt a file:

``` sh
$ gpg --decrypt --output <DECRYPTED-FILE> <FILE>.asc
```

Sign a file with detached signature file:

``` sh
$ gpg --armor --detach-sign <FILE>
$ file <FILE>.asc
<FILE>.asc: PGP signature Signature (old)
```

Verify a signature:

``` sh
$ ls <FILE>*
<FILE>  <FILE>.asc
$ gpg --verify <FILE>.asc
```

Encrypt file symmetrically, i.e. don't use gpg keys, but a use secret password:

``` sh
$ gpg --armor --symmetric <FILE>
$ file <FILE>.asc
<FILE>.asc: PGP message Symmetric-Key Encrypted Session Key (old)
```
