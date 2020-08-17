---
title: GPG
last-changed: <time>2020-07-21</time>
knowledgebase: true
categories: [Linux]
---
## Links

* [The GNU Privacy Guard](https://gnupg.org) <time>2020-04-04</time>
* [Wikipedia: OpenPGP](https://de.wikipedia.org/wiki/OpenPGP) <time>2020-04-04</time>

* [Hauke Laging: Verschlüsselung und Signatur](http://www.hauke-laging.de/sicherheit/einfuehrung.html) <time>2018-05-06</time>
* [Hauke Laging: OpenPGP / GnuPG](http://www.hauke-laging.de/sicherheit/openpgp.html) <time>2018-05-06</time>
* [Phil's PGP Docs](https://www.phildev.net/pgp) <time>2018-05-06</time>
* [OpenPGP Best Practices](https://riseup.net/en/security/message-security/openpgp/best-practices) <time>2018-05-06</time>
* [Mike English: Getting Started with GNU Privacy Guard](https://spin.atomicobject.com/2013/09/25/gpg-gnu-privacy-guard) <time>2018-05-06</time>
* [Creating the perfect GPG keypair](https://alexcabal.com/creating-the-perfect-gpg-keypair) <time>2018-05-06</time>

* [Arch: GnuPG](https://wiki.archlinux.org/index.php/GnuPG) <time>2019-07-13</time>
* [GPG Anleitung](https://wiki.kairaven.de/open/krypto/gpg/gpganleitung) <time>2018-05-06</time>

* [Using an offline GnuPG master key](https://incenp.org/notes/2015/using-an-offline-gnupg-master-key.html) <time>2018-05-06</time>
* [OpenPGP User ID Comments considered harmful](https://debian-administration.org/users/dkg/weblog/97) <time>2018-05-06</time>
* [Nitrokey - Secure your digital life](https://www.nitrokey.com/de) <time>2018-05-06</time>
* [FSFE - Card with subkeys using backups](http://wiki.fsfe.org/TechDocs/CardHowtos/CardWithSubkeysUsingBackups) <time>2018-05-06</time>
* [The GnuPG Smartcard HOWTO](https://www.gnupg.org/howtos/card-howto/en/smartcard-howto.html) <time>2018-05-06</time>
* [Github: jaymzh / pius](https://github.com/jaymzh/pius) <time>2018-05-06</time>
* [Monkeysign](https://monkeysign.readthedocs.io) <time>2018-05-06</time>
* [OpenKeychain](https://www.openkeychain.org) <time>2018-05-06</time>

* [OpenPGP](https://www.openpgp.org) <time>2019-07-13</time>
* [keys.openpgp.org](https://keys.openpgp.org) <time>2019-07-13</time>

* <https://davesteele.github.io/gpg/2014/09/20/anatomy-of-a-gpg-key/>
* <https://debian-administration.org/users/dkg/weblog/105>
* <https://begriffs.com/posts/2016-11-05-advanced-intro-gnupg.html>
* <https://security.stackexchange.com/questions/14718/does-openpgp-key-expiration-add-to-security>
* <https://gist.github.com/grugq/03167bed45e774551155>
* <https://pthree.org/2015/11/19/your-gnupg-private-key>
* <https://www.gesellschaft-zur-entwicklung-von-dingen.de/blog/GnuPG_richtig_einsetzten.html>
* <https://github.com/balos1/easy-gpg-to-paper>
* <https://www.unitas-network.de/support/wiki/smartcards/openpgp-card-initialisieren/#zustzliche-identitten>

## TODO

* <https://jacksum.loefflmann.net/en/index.html>

```sh
$ wget https://sourceforge.net/projects/jacksum/files/jacksum/jacksum-1.7.0.zip/download
$ java -jar jacksum.jar -a crc24 -X -q 00040F13C0F8A16C4CA8F53C0F15295DD887892FF56B
1FC7CA	22
```

## Terms

DSA
: Digital Signature Algorithm

GPG
: GNU Privacy Guard

HSM
: Hardware Security Module

PGP
: Pretty Good Privacy

PKI
: Public Key Infrastructure

RSA
: Rivest-Shamir-Adleman Cryptosystem

WoT
: Web of Trust

## Daily Usage

Encrypt a file:

```sh
$ gpg -a|--armor -e|--encrypt -r|--recipient <KEYID>|<MAIL> <FILE>
$ ls <FILE>*
<FILE>  <FILE>.asc
$ gpg -e|--encrypt -r|--recipient <KEYID>|<MAIL> <FILE>
$ ls <FILE>*
<FILE>  <FILE>.asc  <FILE>.gpg
$ shred -u <FILE>
$ file <FILE>.asc <FILE>.gpg
<FILE>.asc: PGP message Public-Key Encrypted Session Key (old)
<FILE>.gpg: PGP RSA encrypted session key - keyid: 18DDC17A E871446F RSA (Encrypt or Sign) 2048b .
```

Decrypt a file:

```sh
$ gpg -d|--decrypt -o|--output <DECRYPTED-FILE> <FILE>.asc
```

Sign a file with detached signature file:

```sh
$ gpg -a|--armor -b|--detach-sign <FILE>
$ ls <FILE>*
<FILE>  <FILE>.asc
$ gpg -b|--detach-sign <FILE>
$ ls <FILE>*
<FILE>  <FILE>.asc  <FILE>.sig
$ file <FILE>.asc <FILE>.sig
<FILE>.asc: PGP signature Signature (old)
<FILE>.asc: data
```

Verify a signature:

```sh
$ ls <FILE>*
<FILE>  <FILE>.asc
$ gpg --verify <FILE>.asc
```

Encrypt a file symmetrically, i.e. don't use gpg keys, but use a secret
password:

```sh
$ gpg -a|--armor -c|--symmetric <FILE>
$ ls <FILE>*
<FILE>  <FILE>.asc
$ gpg -c|--symmetric <FILE>
$ ls <FILE>*
<FILE>  <FILE>.asc  <FILE>.gpg
$ file <FILE>.asc <FILE>.gpg
<FILE>.asc: PGP message Symmetric-Key Encrypted Session Key (old)
<FILE>.gpg: GPG symmetrically encrypted data (AES256 cipher)
```

### Work with keys locally from keyring

List public keys:

```sh
$ gpg -k|--list-keys <KEYID>|<MAIL>|<NAME>
```

List private keys:

```sh
$ gpg -K|--list-secret-keys
```

Show fingerprint of a key:

```sh
$ gpg --fingerprint <KEYID>|<MAIL>|<NAME>
```

### Work with keys from keyserver

Since SKS pool keyservers have some fundamental problems, use
`keys.opengpg.org` instead.

```text
keyserver hkps://keys.openpgp.org
```

There are several methods to search for keys and import them.

* Manually search for a key and after confirmation import it into keyring.
* Directly import a key without confirmation into keyring.
* Automatically locate and import key as needed. Preferred method for me in
  combination with `keys.opengpg.org`, since there is exactly one key to a mail
  published.

```sh
$ gpg --search-keys <KEYID>|<MAIL>|<NAME>
$ gpg --recv-keys <KEYID>
$ gpg --auto-key-locate keyserver --locate-keys <MAIL>
```

Delete key after confirmation from keyring:

```sh
$ gpg --delete-keys <KEYID>|<MAIL>|<NAME>
```

Get key updates:

```sh
$ gpg --refresh-keys
$ gpg --recv-keys <KEYID>
```

Problem with `--refresh-keys` is, that you expose which keys you have in your
keyring. Consider updating key one by one. Check out parcimonie for it.

### Conversion binary vs. ASCII format

Conversion from binary to ASCII format and from ASCIIi to binary format for
e.g. public keys can be done as follows:

```sh
$ gpg --output <KEYID>.pub.asc --enarmor <KEYID>.pub.gpg
$ sed -i -e '/^Comment:/d' -e 's|ARMORED FILE|PUBLIC KEY BLOCK|' <KEYID>.pub.asc

$ gpg --output <KEYID>.pub.gpg --dearmor <KEYID>.pub.asc
```

## gpg2

### Executables

* gpg
* gpgv
* gpgsm
* gpg-agent
  - Daemon to manage private keys, backend for gpg and gpgsm
* gpgconf
* dirmngr
  - Accessing openpgp keyservers (gpg since version 2.1)
  - Managing crls
  - Providing access to ocsp providers
* dirmngr-client
  - access dirmngr services

### Environment variables

`GNUPGHOME`
: If set, used instead of `~/.gnupg`

`GPG_AGENT_INFO`
: Obsolete (gpg since version 2.1)

### Files

`crls.d`
: Storing cached CRLs (dirmngr)

`dirmngr.conf`
: Config file (dirmngr)

`gpg.conf`
: Config file (gpg)

`gpg-agent.conf`
: Config file (gpg-agent)

`opengpg-revocs.d`
: Contains pregenerated revocation certificates; if primary private key is not
  stored on disk, also move away these revocation certificates (gpg)

`private-keys-v1.d`
: where private keys are stored (gpg-agent)

`pubring.gpg`
: public keyring (gpg before version 2.1)

`pubring.kbx`
: public keybox (gpg since version 2.1)

`random_seed`
: File used to preserve the state of the internal random pool (gpg)

`secring.gpg`
: Secret keyring (gpg before version2.1, newer versions use gpg-agent / private-keys-v1.d)

`sshcontrol`
: Used, if support for ssh agent protocol is enabled (gpg-agent)

`S.gpg-agent`
: Socket (gpg-agent)

`trustdb.gpg`
: Contains trust database, don't backup this file, but use
  `--export-ownertrust` (gpg)

### `gpg.conf`

```text
# Default key to use for signing
default-key <KEYID>

# Key to encrypt with, if no recipient is given
default-recipient <KEYID>

# Key to encrypt to additionally to other recipients
#encrypt-to <KEYID>
# Same but hide this Key ID
hidden-encrypt-to <KEYID>

# Display fingerprint in compact way instead of key ID
keyid-format none
# Would show fingerprint in non-compact way
#with-fingerprint
# Display fingerprints for subkeys
with-subkey-fingerprint
# To compare private keys with/inside private-keys-v1.d
with-keygrip
# Print key listings delimited by colons, useful for scripts/machine parsing
#with-colons

# Avoid information leaked
no-emit-version
no-comments

# Don't include keyids
# Pro:    Doesn't reveal information unnecessarily
# Contra: May slow down decryption on receiving side, since it will try all
#         private keys
#   for all keys
#throw-keyids

# List options
list-options show-keyring,show-policy-urls,show-notations,show-uid-validity,show-sig-expire,show-unusable-uids,show-unusable-subkeys

# Deprecated, use keyserver in dirmngr.conf instead
#keyserver <KEYSERVER>
# Options for all defined keyservers
keyserver-options no-honor-keyserver-url include-revoked

# Preferences/usage of algorithms used
default-preference-list SHA512,SHA384,SHA256,SHA224,AES256,AES192,AES,CAST5,BZIP2,ZLIB,,ZIP,Uncompressed
personal-digest-preferences SHA512,SHA384,SHA256,SHA224
personal-cipher-preferences AES256,AES192,AES,CAST5
personal-compress-preferences BZIP2,ZLIB,ZIP,Uncompressed
# SHA1 is still the only algorithm to generate V4 fingerprints, so we can't
# disable it completely
#weak-digest SHA1
```

### `dirmngr.conf`

```text
# Built-in default hkps://hkps.pool.sks-keyservers.net
keyserver hkps://hkps.pool.sks-keyservers.net
```

### `gpg-agent.conf`

```text
# 5h -> 18000s
default-cache-ttl 18000
default-cache-ttl-ssh 18000

# 24h -> 86400s
max-cache-ttl 86400
max-cache-ttl-ssh 86400

ignore-cache-for-signing
#pinentry-program /usr/bin/pinentry-gnome3
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

### Format

* secret key packet
* user ID packet
* signature packet
* secret sub key packet
* signature packet
* secret sub key packet
* signature packet
* secret sub key packet
* signature packet

* public key packet
* user ID packet
* signature packet
* public sub key packet
* signature packet
* public sub key packet
* signature packet
* public sub key packet
* signature packet

Algo

Epoch time

```sh
$ date -d @1591685390
```

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
  - Only bring it online for key signing purposes and mailing signed keys.
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

To start mount the encrypted (e.g. with LUKS) USB device, where all keys will
be stored, especially the primary key.

Use a directory on this encrypted usb device as home directory for `gpg` either
via environment variable

```sh
$ export GNUPGHOME=<USBDEVICE>/gnupg
```

or use `--homedir=<USBDEVICE>/gnupg` for every following gpg command.

Then generate the primary key:

```sh
$ gpg --version
gpg (GnuPG) 2.2.5
[...]
Home: <USBDEVICE>/gnupg
[...]
$ gpg --expert --full-generate-key
(8) RSA (set your own capabilities)

Toggle on only sign and certify capability

4096
2y

<NAME>
<MAIL>
# no comment
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

* Previously the suggestion was, that the first UID in the generating step
  above should be set without E-Mail, because E-Mail addresses could get
  invalid in future and so we had one UID that remained valid, even if all
  E-Mail addresses would have been changed in future.
* But with using `keys.openpgp.org` as keyserver, that doesn't make sense
  anymore, since it only distributes UID with E-Mail address, because it relies
  on user validation of UIDs before distributing them.
* The comment field we leave empty, because it doesn't provide so much benefit.
* After generating the primary key we can add additional name/E-Mail pairs.

Passphrase:

* Use long passphrase, but avoid too many special characters.
* A 7 word passphrase generated with a diceware list should provide a
  reasonable secure passphrase.

Entropy for key generation:

* Much entropy is needed, maybe you need to move around with the mouse or use
  the keyboard to create enough entropy.
* Tails distribution uses havegd, which helps to create enough entropy.

Add additional name/E-Mail pair:

```sh
$ gpg --edit-key <KEYID>
gpg> adduid
<NAME>
<E-MAIL>
gpg> save
```

Normally the last UID created will be the primary UID used by `gpg`. If you
want to use another UID mainly, set it as primary UID:

```sh
$ gpg --edit-key <KEYID>
gpg> uid <X>
gpg> primary
gpg> save
```

To add subkeys repeat the following steps for each subkey, that you want to
generate:

```sh
$ gpg --expert --edit-key <KEYID>
gpg> addkey
(8) RSA (set your own capabilities)

Toggle on exactly one of sign, encrypt or authenticate capability

2048
6m
gpg> save
```

Remove the primary key to create laptop keys only having the subkeys:

```sh
$ gpg --output <KEYID>.ssk.gpg --export-secret-subkeys <KEYID>
$ # Only do the following step, if you have a backup of your primary key!
$ gpg --delete-secret-keys <KEYID>
$ gpg --import <KEYID>.ssk.gpg
$ shred -vu <KEYID>.ssk.gpg
```

Now you can set a different passphrase (maybe only 5 instead of 7 word
passphrase) to these subkeys to make daily usage easier:

```sh
$ gpg --edit-key <KEYID>
gpg> passwd
gpg> save
```

To check, if creating a keyring with only the subkeys was successful, list the
keys:

```sh
$ gpg --list-secret-keys <KEYID>
```

* `sec#` in output denotes the missing primary key.
* `ssb>` would stand for subkeys on a smartcard.

If you don't use a HSM (e.g. Nitrokey), export the keys to be used on the
laptop:

```sh
$ gpg --armor --output <KEYID>_laptop.pub.asc --export <KEYID>
$ gpg --armor --output <KEYID>_laptop.sec.asc --export-secret-keys <KEYID>
```

Be sure to now use the standard home directory for `gpg`. Then import the
laptop keys and set the trust:

```sh
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

```sh
$ gpg [--keyserver hkps://keys.openpgp.org] --send-keys <KEYID>
```

### HSM - Nitrokey

If you already have set up laptop keys, you can transfer them to a HSM (e.g.
Nitrokey), so that the secret keys are no longer stored on the laptop itself.

If you didn't set up laptop keys, start with the keyrings only containing the
subkeys, where the primary key was already removed.

Setup the HSM:

```sh
$ gpg --card-status
$ gpg --card-edit
gpg/card> admin
gpg/card> passwd
3 - change Admin PIN
1 - change PIN
gpg/card> name
gpg/card> lang
gpg/card> sex
gpg/card> url
http://<YOUR-DOMAIN>/gpg/<KEYID>.pub.gpg
gpg/card> login
gpg/card> quit
```

Now you can transfer the subkeys to the HSM itself:

```sh
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

```sh
$ gpg --card-edit
gpg/card> admin
gpg/card> factory-reset
gpg/card> quit
```

After transfering the keys to a HSM and you want to use the HSM the first time
on a laptop you first need to fetch the public key from a keyserver. If you
have set the url in the HSM earlier, you can use the `fetch` command too:

```sh
$ gpg --card-edit
gpg/card> fetch
gpg/card> quit
$ gpg --edit-key <KEYID> trust quit
5 = I trust ultimately
y
$ gpg --refresh-keys <KEYID>
$ [cd ~/.gnupg/private-keys-v1.d && rm <KEYGRIP>.key # 3x]
```

Again, since this is your own key, you should set the trust to ultimate.

### Backup GPG keys

Creating backups of the gpg key itself only makes sense on the keyring that
contains the primary key, not on the laptop keys nor on the HSM keys.
Ownertrust backup instead makes also sense for the later two.

Create backup of private key (for usage with `paperkey`):

Use --export-options export-backup (it also saves trust packets)


TODO: why not export-minimal or without option? -> all old self signatures are dropped!?
TODO: needed for local sigs or sigs not send to keys.opengpg.org --export-options export-local
    export-local-sigs

TODO: --import-options import-restore

TODO: --import-options import-local import-local-sigs
test with paperkey export-minimal vs export-backup

* backup public keys with own signatures (since keys.openpgp.org doesn't distribute them anymore)
  - maybe just local signs?
* With keys.openpgp.org how is key transition done?
* After creation backup once? Or everytime the key changes?
  - Self signatures should be retrievable from keys.openpgp.org?!
  - Maybe password changes make new backup necessary.

```sh
$ gpg --output <KEYID>.sec.gpg --export-secret-keys --export-options export-backup <KEYID>
$ gpg --output <KEYID>.sec.gpg --export-secret-keys --export-options export-minimal <KEYID>
```

Create backup of public key (e.g. to put on webserver):

```sh
$ gpg --armor --output <KEYID>.pub.asc --export <KEYID>
$ [gpg         --output <KEYID>.pub.gpg --export <KEYID>]
```

Optionally create ASCII armored backups and revocation certificate:

```sh
$ gpg --armor --output <KEYID>.sec.asc --export-secret-keys <KEYID>
$ gpg --armor --output <KEYID>.rev.asc --gen-revoke <KEYID>
1 = Key has been compromised
```

Create paper copies with own script, that uses paperkey and qrencode:

```sh
$ gpg2qrcode -k <KEYID>
$ lpr <KEYID>.sec.pdf
```

Basically the script does the following:

```sh
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

```sh
$ gpg --export-ownertrust > otrust.txt
```

### Restore GPG keys

To restore keys you will need the public key, e.g. from your webserver or a key
server, and the scan of the paper copy of your key:

```sh
$ ls <KEYID>.pub.gpg
$ qrcode2gpg -i <SCAN>.pdf -k <KEYID>
$ ls <KEYID>.sec.recovered
```

If you have to do that manually, i.e. without the script `qrcode2gpg`, simply
type in the paperkey text version into `<KEYID>.sec.recovered.txt`. Then
reconstruct the key with `paperkey`:

```sh
$ paperkey --pubring <KEYID>.pub.gpg --secrets <KEYID>.sec.recovered.txt \
  --output <KEYID>.sec.recovered
```

Decide which home directory for `gpg` you want to use. Then you can restore
your key:

```sh
$ echo $GNUPGHOME
$ gpg --import <KEYID>.sec.recovered
$ gpg --edit-key <KEYID> trust quit
5 = I trust ultimately
y
```

Since this is your own key, you should give ultimate trust.

Import owner trust:

```sh
$ gpg --import-ownertrust < otrust.txt
```

### Maintenance

Before any maintenance task, think about on which keyring you want to operate,
e.g. the one on your encrypted USB device. Create new backups afterwards, if
necessary.

```sh
$ export GNUPGHOME=<USBDEVICE>/gnupg
```

Delete a key from the keyring:

```sh
# Only do the following, if you have backups of your own keys
$ gpg --delete-secret-keys <KEYID>
$ gpg --delete-keys <KEYID>
$ [gpg --delete-secret-and-public-key <KEYID>]
```

Reimport key to the keyring

```sh
$ gpg --import <KEYID>.sec.gpg
```

Change passphrase:

```sh
$ gpg --edit-key <KEYID>
gpg> passwd
gpg> save
```

Set new expiry date:

```sh
$ gpg --edit-key <KEYID>
gpg> key <X>
gpg> expire
gpg> key <X>
gpg> save
$ gpg --send-keys <KEYID>
```

Send every public subkey!

If you want to revoke a key, be sure if you really want to do that. You can't
take back a revocation!

```sh
$ gpg --import <KEYID>.rev.asc
$ gpg --send-keys <KEYID>
```

Check secret key encryption values:

```sh
$ gpg --list-packets ~/.gnupg/secring.gpg
```

* algo 3: CAST5
* algo 9: AES256
* hash 2: SHA1
* hash 10: SHA512

Edit secret key encryption values: no longer works with new private key format

```sh
$ gpg --s2k-cipher-algo AES256 --s2k-digest-algo SHA512 \
  --s2k-mode 3 --s2k-count 65000000 --edit-key <KEYID>
gpg> passwd
gpg> save
```

Recheck secret key encryption values:

```sh
$ gpg --list-packets ~/.gnupg/secring.gpg
```

## Keysigning and Web of Trust

With keys of others you can/should do two different things.

* First you check, that the key you download e.g. from a keyserver is actually
  from the person you think it is (**keysigning**).
* Second you assign a trust value to such a key, that means how much you trust
  another person, how careful he is with his own keysigning (**web of trust**).

Keys that you have signed (where you have checked identity of the owner) you
can use right off. For persons (e.g. person B), which you have not met
personally, you rely now on the web of trust. Maybe others (e.g. person A),
whose key you have directly signed, have signed the wanted key from person B by
themselves. Then it depends on the trust value you assigned to the signed key
of A, if `gpg` also accepts the key of person B.

There are different level, that state how careful you have checked before
signing another persons key:

* (0) I will not answer.
* (1) I have not checked at all.
* (2) I have done casual checking. (Normally used after keysigning parties)
* (3) I have done very careful checking.

Then for the web of trust there are different trust level:

* \- unknown
* n never trust
* m marginal trust (normally used)
* f full
* u ultimate (only for own key)

An unknown key in your keyring signed from one person with full trust or signed
from three persons with marginal trust is now trusted by `gpg`.

You can do the keysigning manually or use some tool like `pius`, `monkeysign`
or `caff`.

### Keysigning - Manual

Before any signing task, you need your primary key, the one that you saved e.g.
on your encrypted USB device.

```sh
$ export GNUPGHOME=<USBDEVICE>/gnupg
```

Now get the key you want to sign:

```sh
$ gpg --search-keys <KEYID>|<MAIL>|<NAME>
$ gpg --recv-keys <KEYID>|<MAIL>|<NAME>
```

List the key with signatures:

```sh
$ gpg --list-sigs <KEYID>
```

Before signing, compare fingerprints of the key you downloaded with the one on
the keysigning papersheet to be sure you will sign the correct key:

```sh
$ gpg --fingerprint <KEYID>
```

Now sign the key:

```sh
$ gpg --edit-key <KEYID>
gpg> sign
(2) I have done casual checking.
gpg> save
```

Now either send the signed key back to a keyserver:

```sh
$ gpg --send-keys <KEYID>
```

But it's better to export the key, encrypt for the owner and send it to him via
E-Mail. That ensures that the owner indeed has access to the E-Mail he was
claiming to possess:

```sh
$ gpg --armor --export <KEYID> | gpg --armor --encrypt --recipient <KEYID> - |
  mutt -s "Your signed key" <EMAIL>
```

The user now has to import his key, that you signed, and should upload it to
keyserver by himself:

```sh
$ gpg --decrypt <ENCRYPTED_KEY> | gpg --import
$ gpg --send-keys <KEYID>
```

### Keysigning - PIUS

For keysigning tasks (individually signing each uid and mail to it's owner) you
can use `pius` (PGP Individual UID Signer).
