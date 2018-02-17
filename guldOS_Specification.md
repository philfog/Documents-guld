
![guldlogo](https://github.com/Alexstang/branding/blob/master/guldOSLogo.jpg)

# guld Operating System (guldOS) Specification


**Author:** 

Ira Miller <public@iramiller.com>

License: CC-BY-4

DRAFT

v0.0.1


## Overview


The guld Operating System (guldOS) is a security and privacy focused variant of [Gentoo](http://gentoo.org/) Linux. guld uses Gentoo’s package manager (portage) to compile everything from source. The goal of guldOS is to compile a minimum build for each user to do secure signing operations in an environment they can trust.

## Disk Encryption

The most distinct functionality of guldOS is around disk encryption. The recommended lowest level filesystem configuration for guldOS is dm-crypt LUKS with a PGP encrypted, detached [keyfile](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption).

The keyfile and/or PGP key must be on a second device, preferably a dedicated smart card with [openPGP](http://openpgp.org/) support. In the simplest implementation, this can be a separate partition or a USB drive.

![guldOSDiskEncrypt](https://github.com/Alexstang/branding/blob/master/guldOS_DiscEncryption.jpg)

[Yubikey](http://yubico.com/) is probably the most popular example of an openPGP smart card, but [Ledger Wallet](https://www.ledgerwallet.com/) also supports cryptocurrency key management, which many guldOS users will find helpful.

Regardless of what openPGP device is used to unlock the PGP key, the key should then be used to decrypt the LUKS keyfile. The LUKS keyfile is then used to decrypt the partition, which can then be mounted for the user.


Single Password Environment
Standardization around PGP allows users to fully encrypt their disk using the same key and password that they use at higher levels, such as guldFS, as well as with applications like password manager [`pass`](https://www.passwordstore.org/) and with email. This creates an easily managed single password security system for all of the various layers of encryption a user might interact with.

![guldOSsinglePWenviro](https://github.com/Alexstang/branding/blob/master/guldOS_SinglePwEnviroment.jpg)

Maintaining this Single Password Environment drives many of the design and application choices throughout guldOS. While it does create a single point of failure, it is the most familiar and practical to expect end users to be able to follow. Furthermore, PGP is so widely supported that the ecosystem of hardware and software options for `openPGP Device` is without equal.

## Portage Overlay

After the crypto layers are configured, guldOS can be treated like any other portage [overlay](https://overlays.gentoo.org/). The specific choices of applications are to always use source distributions and GPG if possible. Specific applications which are to be emerged are:
 * git
 * GnuPG
 * ledger-cli
 * pass
 * xfce4
 * Offlineimap
 * Evolution
 * guldFS
 * Bitcoin (fork luke-jr build to create plain core)
 * Ethereum
 * Dash (package needed)

The full list is an order of magnitude longer when including libraries and such (i.e. Berkeley DB 4.8), but these are the most important packages. Some P2P network graphics and guld branding rounds out the user experience

![TigoCTMSS](https://github.com/Alexstang/branding/blob/master/TigoCTM_prototypeSS.jpg)

TigoCTM Prototype Screenshot
