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

Encrypt a file symmetrically, i.e. don't use gpg keys, but a use secret
password:

``` sh
$ gpg --armor --symmetric <FILE>
$ file <FILE>.asc
<FILE>.asc: PGP message Symmetric-Key Encrypted Session Key (old)
```

## GPG fundamentals

Features of GPG keys:

C
: Certify

E
: En-/decrypt

S
: Sign

A
: Authenticate

### Procedure for creating and using GPG keys

1. Create a key policy.
2. Generate secure GPG keys in a secure environment.
3. Separate primary key and subkeys, i.e. create so called laptop keys.
4. If possible use a HSM (e.g. Nitrokey), instead of laptop keys.

### Key policy

Create a key policy and publish it, e.g. on your site
`http://<YOUR-DOMAIN>/gpg/<KEYID>__policy.1.html`.

## Secure GPG keys

### Thoughts about secure environment

* Semi secure:
  - Store primary key on encrypted usb device.
  - Only bring it online for key signing purposes.
  - Have subkeys on HSM (e.g. Nitrokey) and use them from there for daily
    purposes.
* More secure:
  - Boot USB live medium (e.g. tails).
  - Have and use primary key only offline in USB live medium.

### Recommend GPG keys

Use one primary key of type RSA, keylength 4096 with features CS. Then use 3
subkeys of type RSA and keylength 2048. One key should have feature E, the
second feature S and the third feature A.

### Generate GPG keys

To start mount the encrypted (e.g. with luks) usb device, where all keys will
be stored, especially the primary key.

Use a directory on this encrypted usb device as home directory for `gpg` either
via environment variable

``` sh
$ export GNUPGHOME=<USBDEVICE>/gnupg
```

or use `--homedir=<USBDEVICE>/gnupg` for every following gpg command.

Then generate the primary key:

``` sh
$ gpg --version
gpg (GnuPG) 2.2.7
[...]
Home: <USBDEVICE>/gnupg
[...]
$ gpg --expert --full-generate-key
(8) RSA (set your own capabilities)

Toggle on only sign and certify capability

4096
2y

<NAME>
```

Primary key:

* The primary key is only used to certify (and sign).
* Toggle off the features encryption and authentication, if not already done.
* Signing is done in rare cases to prove ownership of this key to someone, who
  doesn't recognize subkeys.
* After generating the primary key we will add three different subkeys with
  different features encryption, signing and authentication.

Key length:

* For primary key we use the longest, that is possible (4096).
* For subkeys we use 2048.

Expiration date:

* Even though we intend to use the key as long as possible we want to set an
  expiration date, as we can always extend that expiration date in future. But
  if anything happens to the key (getting lost, forgetting passphrase, ...)
  it's ensured, that the key won't get used after that date.
* We use 2 years for the primary key and 6 months for subkeys.

UIDs:

* The first UID in the generating step above we set without E-Mail, because
  E-Mail addresses can get invalid in future and so we have on UID that remains
  valid, even if all E-Mail addresses would be changed in future.
* The comment field we leave empty, because it doesn't provide so much benefit.
* After generating of the primary key we add additional name/E-Mail pairs.

Passphrase:

* Use long passphrase, but avoid too many special characters.
* A 7 word passphrase generated with a diceware list provides a reasonable
  secure passphrase.

Entropy for key generation:

* Much entropy is needed, maybe you need to move with the mouse or use the
  keyboard to create enough entropy.
* Tails distribution uses havegd, which helps to create enough entropy.


Add additional name/E-Mail pair:

``` sh
$ gpg --edit-key <KEYID>
gpg> adduid
<NAME>
<E-MAIL>
gpg> save
```

Normally the last UID created will be the primary UID used by `gpg`. If you
want to use another UID mainly, set it as primary UID:

``` sh
$ gpg --edit-key <KEYID>
gpg> uid <X>
gpg> primary
gpg> save
```

To add subkeys repeat the following steps for each subkey, that you want to
generate:

``` sh
$ gpg --expert --edit-key <KEYID>
gpg> addkey
(8) RSA (set your own capabilities)

Toggle on exactly one of sign, encrypt or authenticate capability

2048
6m
gpg> save
```

Remove the primary key to create laptop keys only having the subkeys:

``` sh
$ gpg --output <KEYID>.ssk.gpg --export-secret-subkeys <KEYID>
$ # Only do the following step, if you have a backup of your primary key!
$ gpg --delete-secret-keys <KEYID>
$ gpg --import <KEYID>.ssk.gpg
$ shred -vu <KEYID>.ssk.gpg
```

Now you can set a different passphrase (maybe only 5 instead of 7 word
passphrase) to these subkeys to make daily usage easier:

``` sh
$ gpg --edit-key <KEYID>
gpg> passwd
gpg> save
```

To check, if creating a keyring with only the subkeys was successful, list the
keys:

``` sh
$ gpg --list-secret-keys <KEYID>
```

* `sec#` in output denotes the missing primary key.
* `ssb>` would stand for subkeys on a smartcard.

If you don't use a HSM (e.g. Nitrokey), export the keys to be used on the
laptop:

``` sh
$ gpg --armor --output <KEYID>_laptop.pub.asc --export <KEYID>
$ gpg --armor --output <KEYID>_laptop.sec.asc --export-secret-keys <KEYID>
```

Be sure to now use the standard home directory for `gpg`. Then import the
laptop keys and set the trust:

``` sh
$ unset GNUPGHOME
$ gpg --import <KEYID>_laptop.pub.asc
$ gpg --import <KEYID>_laptop.sec.asc
$ gpg --edit-key <KEYID> trust quit
5 = I trust ultimately
y
```

Since this is your own key, you should give ultimate trust.

Now you can publish the public key, either by putting it on your webserver or
by sending it to a keyserver:

``` sh
$ gpg --send-keys <KEYID>
```

### Backup GPG keys

Create backup of private key (for usage with `paperkey`):

``` sh
$ gpg --output <KEYID>.sec.gpg --export-secret-keys <KEYID>
```

Create backup of public key (e.g. to put on webserver):

``` sh
$ gpg         --output <KEYID>.pub.gpg --export <KEYID>
$ gpg --armor --output <KEYID>.pub.asc --export <KEYID>
```

Optionally create ASCII armored backups and revocation certificate:

``` sh
$ gpg --armor --output <KEYID>.sec.asc --export-secret-keys <KEYID>
$ gpg --armor --output <KEYID>.rev.asc --gen-revoke <KEYID>
1 = Key has been compromised
```

Keep the following files secret and safe:

* `<KEYID>.sec.gpg` (needed for usage with `paperkey`)
* `<KEYID>.sec.asc`
* `<KEYID>.rev.asc`

Create paper copies with a script that uses paperkey and qrencode:

``` sh
$ gpg2qrcode -k <KEYID>
$ ls <KEYID>.sec.pdf
```

### Restore GPG keys

To restore keys you will need the public key, e.g. from your webserver or a key
server, and the scan of the paper copy of your key:

``` sh
$ ls <KEYID>.pub.gpg
$ qrcode2gpg -i <SCAN>.pdf -k <KEYID>
$ ls <KEYID>.sec.recovered
```

Decide which home directory for `gpg` you want to use. Then you can restore
your key:

``` sh
$ echo $GNUPGHOME
$ gpg --import <KEYID>.sec.recovered
$ gpg --edit-key <KEYID> trust quit
5 = I trust ultimately
y
```

Since this is your own key, you should give ultimate trust.