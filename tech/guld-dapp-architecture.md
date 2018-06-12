# Architecture for a guld DAPP 

The general strategy we will recommend for developing guld DAPPs is to clone a selection of branches of the blocktree into the browser's storage. The DAPP will then be able to read, analyze and transform the data in it's raw form, as well as use modern web development tools for the UI.

As a reminder, the blocktree directory structure is:

| local path | description |
|------------|-----------------------------|
| `/BLOCKTREE/<user>` | User's branch of the blocktree, as a dirty, local git repository. |
| `~/ledger/<code>` | User's perspective on the ledger for commodity `<code>` |
| `~/keys/pgp/<username>/<fpr>.asc` | PGP public keys, sorted by username then fingerprint. |
| `~/devices/<username>/<devicename>/etc/hosts` | Official device config file, as signed by device owner. |
| `~/tech/<lang>` | Source code sorted by language. |
| `~/.password-store` | Standard Unix Password Manager data directory. Contents PGP encrypted. |
| `~/blocktree/<username>` | Owner's perspective on another blocktree branch (individual or group) |

## Pre-requisites

There are three big pre-requisites for any guld DAPP: pgp, git, and ledger. For guld games, we had to make all three available in the browser environment, which adds two important restrictions: Javascript, and sandboxed access to resources like the filesystem and local programs.

### PGP

Thankfully, Javascript has a full, audited implementation of OpenPGP called [OpenPGP.js](https://openpgpjs.org/). Since the implementation is quite complete, this library even supports OpenPGP devices like smart cards and hardware wallets.

### Git

For git, the Javascript option are not as complete or robust. Of the native jS (no API to system git) options, most do not have PGP signing functionality implemented, and additionally most are not built with the browser environment in mind.

[Isomorphic-git](https://isomorphic-git.github.io/) is a new implementation, however, which already meets all of our needs, and is on it's way to becoming the best, most complete JS implementation of git. It works in the browser, and implements PGP commit signing and verification.

### Ledger

Ledger is required to consume accounting data, and there is no Javascript implementation. In fact, there are few JS tools related to ledger, and most are poorly maintained. On the other hand, the original ledger-cli implementation runs well on Windows, Linux and Mac, and is very stable.

The recommended method for consuming ledger data from a guld DAPP is using the [ledger-native](https://github.com/guldcoin/ledger-native) bindings, which implement chrome's [native messaging API](https://developer.chrome.com/apps/nativeMessaging). These bindings come with installation instructions for the operating systems listed above, and allow generic access to the system's installed ledger.

``` Javascript
chrome.runtime.sendNativeMessage('com.guld.ledger',
  {'cmd': 'bal', 'stdin': ledgerdata},
  response => {
      console.log(response)
  }
)
```

## Useful additions

### BrowserFS

[BrowserFS](https://github.com/jvilk/BrowserFS) "emulates the nodejs filesystem API." Immediately, this lets us stage our git repositories in browser memory or storage, and interact with them in the normal git fashion. Isomorphic-git provides this sort of API, and is `fs` agnostic, happily accepting BrowserFS instances.

In the long term, a guldFS backend could be written for BrowserFS, allowing more seemless and native integration with the blocktree.

## The guld JS library

The guld JS library uses the above tools to implement the required functionality of guld-ledger and the guld group.

 + Name registration
 + Key management
 + Ledger transaction templates for transfer, grant, and register

## Your DAPP library

The primary contract for your DAPP should be implemented as a separate library. This makes code audits and interaction easy to separate from UI. For example, you should have an agreement like [this one](https://github.com/guld-games/Documents/blob/master/contracts/gg-agreement.md) for [guld games](https://guld.gg). This agreement should define your overall group concensus rules, as well as allowed transformations to the group's data.

If your group has a ledger, custom transaction templates can be defined to suppliment those made availale in the guld JS lib.

## Your DAPP UI

Finally, you can import all of the above and get to work on the frontend. From here on out it is standard web development, with the understanding that the above tools should be used to replace any server.
