# Guld: Universal Protocol for Relative Consensus

## Abstract

A polytree with signed, typed, symmetrical nodes can be used to represent, record, compare, and perform operations on individual and group perspectives on any topic describable in a digitized record. This `blocktree` represents a virtual universe for the participants, including natural causality and one-directional "time". The participants are able to make assertions about each other's perspectives, building proofs of consensus or dissent. Using existing technologies like Git, PGP, and SSH, such a polytree can be constructed in a timely and accessible manner, while maintaining scalability and decentralization at the protocol level.

## Introduction

Guld is a new decentralized internet where users control their own data and digital identity to empower and automate the real human contracts we use everyday.

With security rooted in real human interactions, Guld is a new concept of Cryptographic Network which allows for the human management and validation of identities, addresses, and devices for efficient and secure execution of human contracts.
Identity Management

Guld enables the internet to better support secure execution of human contracts. This human consensus network, with cryptographic security and identity verification, will allow people to take back control of their data and their digital identities.

Since Bitcoin(1) introduced the blockchain concept in 2009, hundreds of experimental technologies have been developed to help users achieve consensus on one topic or another. The majority of these have been organized around the model of a central, unbranching trunk of absolute truth: a blockchain. To "fork" a blockchain is perceived as negative, because it breaks the implicit or explicit contract of absolute truth that formed the chain. Due in part to their unforkability, blockchains have proved inflexible at managing conflict and change. The blocktree is a significantly more flexible approach.

## Namespaces

Guld provides a name-registration service connecting names to digital identities through PGP on a public ledger. We do this through GULD tokens.

GULD tokens are used to register the names on the network, designed for management of contracts. GULD is burned for registrations of other names for blockchains, ERC20 tokens, addresses, and devices (i.e., mobile phones). In addition, GULD is issued in meritocratic fashion to individuals or groups who contribute to the Guld network.

## Witnessing (as entry to the network with a new perspective)

## Signing -> Asymmetrical encryption and Communicating consensus

"I think, therefore I am"(7) is a beautiful and strong proof, but it only works for the observer themselves. A modernization using asymmetric cryptography would be "I sign a unique perspective using this key, therefore I am."

## Ledger

- Branches
- Groups
- Chains
- Public Addresses
- IDs

## Blocktree

Though similar in some ways to the blockchain, our blocktree infrastructure is better in many ways.

Similarities:

- Both are arguably Directed Acyclic Graphs.
- Both are immutable, and each change builds off previous changes over linear time.
- Both are public records. Not all parts of the Guld blocktree are public, however the ledger is public.

The differences:

- A blockchain is linear, whereas a blocktree, in data-structure terms, is a tree.
- The tree has a root and an arbitrary number of branches (folders) and leaves (data items, i.e., files). A given branch can have arbitrary number of sub-branches, and so on, ad infinitum.

Why the blocktree is better than blockchain:

- Blocktree can host an infinite number of blockchains
- Efficient scalability
- Controlled security
- Lower technological overhead cost

### Git

Though the standard `git` software package ships with a server, and that server supports p2p `ssh` authentication, this is rarely made use of. In practice, users tend to use one of the popular hosting services, such as [github](http://github.com), [bitbucket](http://bitbucket.org),  [gitlab](http://gitlab.com), etc.. One of these is more than sufficient for the average open source project, but not for a consensus network. Thankfully, git supports a multiple `remote` hosts for each repository, and the protocol strengthens the user's signed ownership of the state, making each inter-changeable.

``` bash
$ git remote -v
isysd    git@guld.host:isysd/guld.git (fetch)
isysd    git@guld.host:isysd/guld.git (push)
isysd-github    git@github.com:isysd/guld.git (fetch)
isysd-github    git@github.com:isysd/guld.git (push)
isysd-bitbucket    git@bitbucket.org:isysd/guld.git (fetch)
isysd-bitbucket    git@bitbucket.org:isysd/guld.git (push)
cindy-zimmerman    git@guld.host:cindy-zimmerman/guld.git (fetch)
cindy-zimmerman    git@guld.host:cindy-zimmerman/guld.git (push)
```

Because we recognize that `isysd` is a living person who uses a specific PGP key to publish to one or all of the `isysd*` mirrors shown, we can accept the most up to date with his signed commits. The hosts are not to be trusted. Logistically, checking all of these is inefficient, so users should establish P2P socket connections with their friends to send notifications about commits and hosts.

Any mutually accepted users of the protocol could publish encrypted contact info for each other, including IP addresses for git hosts and live socket sessions. Ideally, each user on the network would host their own `git` servers, using software like gitolite(11) to manage permissions in repository, and publishing their IP address (selectively encrypted) all on the blocktree.

#### Example Gitolite configuration

```
@users = u1 u2 u3

repo foo/CREATOR/[a-z]..*
    C   =   u1 u2 u3
    RW+ =   CREATOR
    RW  =   WRITERS
    R   =   READERS
```

This would create a provable hosting service, where the host can demonstrate compliance with community access rules, and even put the configuration itself in the control of one or more signing users.

Running `git` and `ssh` servers is an, as yet, under-assessed security risk to request of end users. Until there is more data from experienced systems administrators, and the protocol has stabilized, end users should not be required to host their own repositories.

Starting with professional, trustless hosting services, the guld network will evolve to proven hosts, finally to a completely p2p, self-hosted network.

This evolution will separate guld more and more from traditional infrastructure like Dynamic Name Servers (DNS), gradually reshaping the internet on P2P terms, with P2P identities, governance, and finances built in.

## DAPPS

## DAOs

## Conclusion

The `blocktree` represents a rich, self-contained world, with causality and enforceable laws for it's participants. By using relative proofs based on perspectives, relative truths and consensus can be reached, lost, and reachieved flexibly, in ways that blockchains are unable to replicate. This blocktree of relative truth, or truth good enough for a community, is superior to blockchains of absolute truth for human contracts, firms, and governments.

## References
https://bitcoin.org/bitcoin.pdf

Adding Applied Examples, and Token Issuance and. Tokens are created through grants, and validated through proof of records.

