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

List public keys:

``` sh
$ gpg --list-keys <KEYID>|<MAIL>|<NAME>
```

List private keys:

``` sh
$ gpg --list-secret-keys
```

Show fingerprint of a key:

``` sh
$ gpg --fingerprint <KEYID>|<MAIL>|<NAME>
```

Show fingerprint of a key file without importing it into the keyring:

``` sh
$ gpg --with-fingerprint <KEYID>.pub.asc
```

Search for (and get) a key:

``` sh
$ gpg --search-keys <KEYID>|<MAIL>|<NAME>
```

Receive a key:

``` sh
$ gpg --recv-keys <KEYID>
```

Refresh keys:

``` sh
$ gpg --refresh-keys [<KEYID>]
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
$ gpg --send-keys <KEYID> [--keyserver hkp://eu.pool.sks-keyservers.net]
```

### HSM - Nitrokey

If you already have set up laptop keys, you can transfer them to a HSM (e.g.
Nitrokey), so that the secret keys are no longer stored on the laptop itself.

If you didn't set up laptop keys, start with the keyrings only containing the
subkeys, where the primary key was already removed.

Setup the HSM:

``` sh
$ gpg --card-status
$ gpg --card-edit
gpg/card> admin
gpg/card> passwd
3 - change Admin PIN
1 - change PIN
gpg/card> name
gpg/card> sex
gpg/card> url
http://<YOUR-DOMAIN>/gpg/<KEYID>.pub.gpg
gpg/card> login
gpg/card> quit
```

Now you can transfer the subkeys to the HSM itself:

``` sh
$ gpg --edit-key <KEYID>
gpg> key X
gpg> keytocard
gpg> key X
gpg> key Y
gpg> keytocard
gpg> key Y
gpg> key Z
gpg> keytocard
gpg> save
```

If you have to reset your HSM (e.g. when admin PIN was entered incorrectly
three times), you can do this with the following steps. Be aware the keys will
be deleted, you should have made backups before even transfering the keys for
the first time to the HSM. From an HSM itself you can't backup keys, since this
would defeat the purpose of a HSM.

``` sh
$ gpg --card-edit
gpg/card> admin
gpg/card> factory-reset
gpg/card> quit
```

After transfering the keys to a HSM and you want to use the HSM the first time
on a laptop you first need to fetch the public key from a keyserver. If you
have set the url in the HSM earlier, you can use the `fetch` command too:

``` sh
$ gpg --card-edit
gpg/card> fetch
gpg/card> quit
$ gpg --edit-key <KEYID> trust quit
5 = I trust ultimately
y
```

Again, since this is your own key, you should set the trust to ultimate.

### Backup GPG keys

Creating backups of the gpg key itself only makes sense on the keyring that
contains the primary key, not on the laptop keys nor on the HSM keys.
Ownertrust backup instead makes also sense for the later two.

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

Create paper copies with own script, that uses paperkey and qrencode:

``` sh
$ gpg2qrcode -k <KEYID>
$ lpr <KEYID>.sec.pdf
```

Basically the script does the following:

``` sh
$ gpg --output <KEYID>.pub.gpg --export <KEYID>
$ gpg --output <KEYID>.sec.gpg --export-secret-keys <KEYID>
$ paperkey --secret-key <KEYID>.sec.gpg --output <KEYID>.sec.paperkey.txt
```

* The public key `<KEYID>.pub.gpg` you can put on your webserver.
* The paperkey `<KEYID>.sec.paperkey.txt` you can print out.
* My own script `gpg2qrcode` qrencodes the paperkey version additionally and
  produces a pdf to printout. This simplifies later the process of restoring.
  Scanning and retrieving the key from the qrcode saves you a lot of typing,
  which is error prone.

Keep the following files secret and safe:

* `<KEYID>.sec.gpg` (needed for usage with `paperkey`)
* `<KEYID>.sec.asc`
* `<KEYID>.rev.asc`
* `<KEYID>.sec.pdf`

Export owner trust:

``` sh
$ gpg --export-ownertrust > otrust.txt
```

### Restore GPG keys

To restore keys you will need the public key, e.g. from your webserver or a key
server, and the scan of the paper copy of your key:

``` sh
$ ls <KEYID>.pub.gpg
$ qrcode2gpg -i <SCAN>.pdf -k <KEYID>
$ ls <KEYID>.sec.recovered
```

If you have to do that manually, i.e. without the script `qrcode2gpg`, simply
type in the paperkey text version into `<KEYID>.sec.recovered.txt`. Then
reconstruct the key with `paperkey`:

``` sh
$ paperkey --pubring <KEYID>.pub.gpg --secrets <KEYID>.sec.recovered.txt \
  --output <KEYID>.sec.recovered
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

Import owner trust:

``` sh
$ gpg --import-ownertrust < otrust.txt
```

### Maintenance

Before any maintenance task, think about on which keyring you want to operate,
e.g. the one on your encrypted USB device. Create new backups afterwards, if
necessary.

``` sh
$ export GNUPGHOME=<USBDEVICE>/gnupg
```

Change passphrase:

``` sh
$ gpg --edit-key <KEYID>
gpg> passwd
gpg> save
```

Set new expiry date:

``` sh
$ gpg --edit-key <KEYID>
gpg> key <X>
gpg> expire
gpg> save
$ gpg --send-keys <KEYID>
```

If you want to revoke a key, be sure if you really want to do that. You can't
take back a revocation!

``` sh
$ gpg --import <KEYID>.rev.asc
$ gpg --send-keys <KEYID>
```

Check secret key encryption values:

``` sh
$ gpg --list-packets ~/.gnupg/secring.gpg
```

* algo 3: CAST5
* algo 9: AES256
* hash 2: SHA1
* hash 10: SHA512

Edit secret key encryption values:

``` sh
$ gpg --s2k-cipher-algo AES256 --s2k-digest-algo SHA512 \
  --s2k-mode 3 --s2k-count 65000000 --edit-key <KEYID>
gpg> passwd
gpg> save
```

Recheck secret key encryption values:

``` sh
$ gpg --list-packets ~/.gnupg/secring.gpg
```
