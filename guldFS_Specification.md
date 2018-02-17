

![guldlogo](https://github.com/Alexstang/branding/blob/master/Guld%20Logo.png)


# guld FileSystem (guldFS) Specification


**Author:** 

Ira Miller <public@iramiller.com>


License: CC-BY-4

DRAFT

v0.0.2 



## Overview

The guld FileSystem (guldFS) is a distributed, signed, encryptable, and version controlled filesystem in user space. guldFS is designed to run in Linux using stable, open source components like git, GnuPG, bittorrent, and FUSE.


## FUSE

guldFS uses Filesystem in Userspace (FUSE) to create a native filesystem experience from the kernel level up.

Local users mount directories using fuse, and make changes like they would edit any other file. The changes are then passed through libfuse to the kernel, and then back out into the local git data directory.

The local git data directory should be kept in a “clean” state, in the fashion of a git server. Also following git server convention, this data should be managed by a system user named `git`.


![Fuseguldfs](https://github.com/Alexstang/branding/blob/master/Fuse_guldfs.jpg)




## Local Git Server

The git server can act only as a local backup of user data, but it can also be configured to synchronize with third party git repositories. To manage the permissions, gitolite is the recommended server.

[Gitolite](http://Gitolite.com) can be configured to allow limited secure shell (SSH) sessions by other users identified by their usernames and corresponding SSH keys.

For example, a group of users `family` could be given read-only access to all subdirectories of your pictures, but not to your work documents. Anyone authenticating themself with an SSH key known to be in the `family` group would be allowed to download a picture from you.

These remote users could be using plain old git repositories, or could also be running guldFS, allowing them to mount your pictures directories on their local system.


![Localgitguldfs](https://github.com/Alexstang/branding/blob/master/Localgitserver_guldfs.jpg)


## Branches and Merging

Each user will have a named branch for each git repository, so there should never be any ambiguity about which branch to checkout or commit to. The middleware can always commit to and pull from branch `isysd` for user `isysd`.

Each repository should have a designated owner, who governs the contents. When the owner merges a commit, the rule is for all user branches to also merge. This process should be managed for all users and all repositories by a service running under the `git` user. Since only merges are to be performed by this service, it does not need an identity or signing key, and only needs write access to the clean copy of each repository.

![branchesguldfs](https://github.com/Alexstang/branding/blob/master/Branches%26Merging_guldfs.jpg)

The owner could be another user on the same local system, or it could be a remote user identified by username and SSH key. All that is required for consensus, and for merging, is that the official owner key(s) have signed the owner branch. Once the threshold of signatures is met, the commit(s) should be merged into all Reader branches.

## guld Network

The guld network is peer to peer and split into two halves: messaging (metadata) and SSH (data). The messaging portion is a pub/sub 0MQ network, where each network node publishes known commit hashes. The SSH portion of the network is for retrieving and pushing the contents of each commit.

![networkguldfs](https://github.com/Alexstang/branding/blob/master/guldnetwork_guldfs.jpg)

The contents of the 0MQ messages should consist of `$user:$repository:$commit-hash`. Because the contents are not sent, and not recoverable based on the hash, these messages are safe to distribute all across the network in a `gossip` pool. The term gossip is used because the messages are not necessarily observed by the sending node, and neither node may be able to access the full contents that the message refers to. Should an interested and authorized user hear about the message however, he need only open an SSH connection to the user in the message, and request to pull down the commit hash.

## Configuration

Each repository should be configured using a version controlled file. The recommended name for this file is `.gap.ini`. The file must, at minimum, describe the path to the repository, and it’s official owner.

``` .gap.ini example
[guldfs]
path = /isysd/pictures/family/
owner = isysd
```

This lets other users know the governance for the repository, and it’s address. This address can be used in local file structures (i.e. mounted at /home/isysd/pictures/family) but that is not required. It is, however, required to name the repository after this path in kebab case (i.e. isysd-pictures-family), and to use the path in all related network messages.


### **_Additional Configuration_**


For additional security, and/or for large file support, it is recommended to use pre and post commit hooks to transform the commit contents. For example, the pre-commit hook might do something like encrypt the files using another user’s PGP key, create a torrent out of the encrypted files, and then commit the torrent to git, ignoring both the plaintext and encrypted files. The reader would have a post-commit hook that would do the reverse, opening the torrent, downloading the encrypted files, then decrypting them to finally access the plaintext file.

![additionalconfigguldfs](https://github.com/Alexstang/branding/blob/master/AdditionalConfiguration_guldfs.jpg)

Because git hooks are arbitrarily programmable, it is not necessary to describe every possible use case. The important thing is that each repository declare pre and post commit hooks in .gap.json, so that all members of that repository follow the same rules.

``` .gap.ini example w/ hooks
[guldfs]
path = /isysd/pictures/family/
owner = isysd
pre-commit = path/to/pre-commit.sh
post-commit = path/to/post-commit.sh
```

### **_Mirror Local Filesystem Permissions_**

Unix-based filesystems have a common permission scheme, broken into three categories: user, group, others. Each of these can have some combination of read (r), write (w), and execute (x) permissions. These same file permissions exist in guldFS, but are enforced through cryptography. This allows the files to be distributed on a network without compromising the access to any file or directory.

The enforcement of various combinations and file sizes requires a combination of pre and post commit hooks as described in the Additional Configuration section. 

**Hook Format Map**


|Format|Min Users|Max Users|Torrent at Size|
|:-----:|:------:|:-------:|:-------------:|
|Plain text|1    |N/A      |10 MB          |
|GPG Encrypt|1   |12       |10 MB          |
|AES Encrypt|2   |N/A      |10 MB          |
|Torrent|1       |N/A      |N/A            |



Depending on how many users are in the group, and how large the file is, one or more hook rules may be in effect. For example, a large, private file for a group of 1 would be GPG encrypted. If a second user was to be added to the group with access to that large file, GPG would still be used, but the GPG encrypted version of the file would then also be torrented.

The guldFS fuse middleware should automatically determine which rules to apply in most cases, based on the table above. In the case where a user wishes to explicitly set additional rules for a directory, this can be done by editing the .gitignore file with markup properties. The syntax to turn on a rule for following lines is: `#guld:<rule1>[:<rule2…]`. For example, to turn on the GPG rule for all following files, add the line `#guld:gpg`. To also include torrenting, use `#guld:gpg:torrent`.


**_File Permission Examples_**

|       |User   |Group  |World |Properties|
|---------|---------|---------|---------|---------|
|/home/\<user>/|6 (+rw)|4 (+r)|4 (+r)|write-check|
|/home/\<user>/tech|7 (+rwx)|5 (+rx)|5 +(r)|write-check|
|/home/\<user>/.password-store|6 (+rw)|4 (+r)|0|GPG, write-check|
|/home/\<user>/Pictures|6 (+rw)|4 (+r)|0|AES/GPG, write-check|
|/home/\<user>/Videos|6 (+rw)|4 (+r)|0|AES/GPG, Torrent, write-check|


In most of these real world examples, only the user has write permission, which needs to be checked. This is as simple as a post-pull hook gpg signature count. If the gpg signature doesn’t match the owner’s, delete the commit. Additionally, either the AES or GPG rule is applied depending on how large the group is, which is contextual. Finally, we make assumptions, like that Pictures are going to be small enough to not require torrenting, but that Videos will almost certainly require it.



