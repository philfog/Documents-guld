

![guldlogo](https://github.com/Alexstang/branding/blob/master/Guld%20Logo.png)


# guld Name Service (gns)


__Authors:__

Ira Miller <public@iramiller.com>

Cindy Zimmerman <cindy@tigoctm.com>

License: CC-BY-4.0

DRAFT

v0.0.1



## Overview


The guld name service aka gns is a decentralized replacement for the legacy Dynamic Name Service (DNS) infrastructure. Peers of the guld network will keep their own and each other’s MAC and IP address history for p2p routing of standard TCP and UDP traffic. guld name service also introduces a new naming and domain routing scheme, with usernames as personal, top level domains.



## Names


Each user and group is required to register a unique, alphanumeric name, which is treated like a top level domain. I.e. if my name is `isysd`, then peers could address me with urls ending in `.isysd`


Individual devices are registered under a top level user, with a set of child keys. Devices have names unique for their owner. For example, user `isysd` might have devices `phone` and `laptop` with individual addresses `phone.isysd` and `laptop.isysd`


### **_Structure of a guld URI_**

`<device name>.<user or group name>/<blocktree path>`


### **_Name ownership_**

All names are issued and governed by the `guld` group. A name is considered registered when greater than 50% of the guld equity recognize it.

Each name must list one or more RSA 2048+ bit pgp keys for signing, and register or be authorized by at minimum one full node. The signing arrangement determines which type of name can be registered.


|Category|Keys|Registration Cost|
|------|------|------|
|device|user or group as parent|0.1 guld|
|user|one parent key|1 guld|
|group|1-10 user(s) as parents|1 guld / member|
|medium group|11-100 users as parents|100 guld|
|large group|101-1,000 users as parents|1,000 guld|
|enterprise group|1,001+ users as parents|10,000 guld|


## Name Resolution

Each user and group is responsible for maintaining an up to date, signed record of their devices, including MAC and IP address history. Additionally, each group must maintain a complete record of all active members.

Optionally, users may maintain each other's records for direct routing.

As a last resort, the guld group should be the parent to all other public areas of the network.

Therefore, each request can be routed using the following priorities.

`local -> friends -> groups -> guld group`


### **_Blocktree Path Resolution_**

Once the device has been resolved, a path resolution process begins. This is the same as http, where a FQDN determines a server, then the path determines which files to load off of said server. For example, say you reach http://google.com, a fully qualified domain name (FQDN). You now need to tell the google.com server(s) which record of theirs you would like, i.e. google.com/images for google images. The part after the top level domain (.com) is the path.

In the guld network, paths are strictly ordered according to the blocktree taxonomy. That is, each host serves paths relative to `/home/<owner name>`. For example, each member is required to have a directory `/devices`. Assuming the user's name is `isysd`, that directory would be locally mounted at `/home/isysd/devices`. If being addressed on the network, the directory could be loaded alternately by `isysd/devices` or `<device name>.isysd/devices`

The top tier of directories is referred to as `b1`, and is also governed by the guld group. guld reserves the right to assign new `b1` names, which may become required branches for some or all guld members. Tentatively, a suggested requirement of 25,000 guld and 66% guld equity approval would be required to register a new `b1` name.


|Blocktree level|Registration Cost|
|---------------|-----------------|
|'b1'        |       25,000 guld  |
|'b2'           |   licensed with `b1`|


After `b1`, paths are governed by the `b1` owner. For example, the guld group owns `b1` record `devices`, and requires the following subdirectory structure: `/devices/<device name>/etc/hosts`. In this case, the device's active `hosts` file is the expected response.

### **_Format_**

The gns routing tables must be formatted as `hosts` files, typically found at `/etc/hosts`. Individual device `hosts` files are concatenated regularly by a local service, creating a local name cache. This cache of friends is the preferred node list for any network queries, including for routing info.

Groups link `hosts` files for all members, including said members' network addresses. Therefore, groups can be treated as a routing table for members.

Individual `hosts` file entries consist of an IP address, whitespace, and then a domain name. For example, most devices will include a localhost domain for routing locally.
### **_Example hosts line_**

`127.0.0.1	localhost`


## Feasibility
### **_User Experience_**

Because `hosts` files are (configurably) used before remote dns resolution, this method provides a seamless UX. Users and programs make dns requests like usual, and the local operating system knows to prioritize local `hosts`records.

### **_Servers_**

While `gns` is designed as a p2p network, its feasible users will want services to complement light devices such as phones. These devices are not capable of maintaining a full node, and therefore, must be associated with one.

Such node hosting services are allowed on the network and may charge whatever fees users agree to, so long as their nodes meet the technical requirements.

